I'm happy to announce a new feature that will be available in Symfony 2.4: You can automatically show your log messages in the console output of commands.
This eases giving feedback in your console commands dramatically. Before I show you how to use this feature, let's take a look at what problem it solves.

When you want to output text to the console you usually used the [`OutputInterface`](http://api.symfony.com/2.3/Symfony/Component/Console/Output/OutputInterface.html) instance
that is passed when your command gets executed like in the following example:


    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $data = $input->getArgument('data');
        $myService = $this->getContainer()->get('my_service');
    
        if ($output->getVerbosity() >= OutputInterface::VERBOSITY_DEBUG) {
            $output->writeln(sprintf('Received data: %s', $data));
        }
    
        if ($output->getVerbosity() >= OutputInterface::VERBOSITY_VERBOSE) {
            $output->writeln('Processing data...');
        }
    
        $returnValue = $myService->process($data, $output);
    
        if ($returnValue) {
            $output->writeln('<info>Succeeded</info>');
        } else {
            $output->writeln('<error>Failed</error>');
        }
    }

But there are two problems:

- It's cumbersome to print information depending on the verbosity settings (-v, -vv, -vvv) because you need to wrap the calls in conditions.
- If the service you call also wants to give feedback you would need to pass on the `$output` as with `$myService->process($data, $output)`. But this is really bad
code in terms of separation of concerns and coupling. What if this service has a dependency on another service? Working with the `OutputInterface` and passing around the instance is not the way to go.

To solve these things I thought it would be much easier to rely on the logger which is highly related. So I added integration between [Symfonys Console Component](http://symfony.com/doc/current/components/console/index.html) 
and Symfonys logging library [Monolog](https://github.com/Seldaek/monolog) in [PR #8167](https://github.com/symfony/symfony/pull/8167). 
It features a new handler for Monolog (`ConsoleHandler`) that listens to the [console events](http://symfony.com/blog/new-in-symfony-2-3-events-in-the-console-component) 
and upon activation will write log messages to the console output depending on the current log level and the console verbosity. By default this mapping between log level and verbosity is as follows:

- `OutputInterface::VERBOSITY_NORMAL` will show all _WARNING_ and higher logs,
- `OutputInterface::VERBOSITY_VERBOSE` ( _-v_ ) will show all _NOTICE_ and higher logs,
- `OutputInterface::VERBOSITY_VERY_VERBOSE` ( _-vv_ ) will show all _INFO_ and higher logs,
- `OutputInterface::VERBOSITY_DEBUG` ( _-vvv_ ) will show all _DEBUG_ and higher logs, i.e. all logs.

Now let's see how we can activate and configure this feature in your Symfony project. With [PR #42](https://github.com/symfony/MonologBundle/pull/42) on the MonologBundle, I made this handler just as configurable as any other
Monolog handler. All it needs is an additional handler of type `console` in your `config_dev.yml` and `config_prod.yml` files.

    monolog:
        handlers:
            main:
                type:   stream
                path:   %kernel.logs_dir%/%kernel.environment%.log
                level:  debug
            console:
                type:   console

With this in place, all messages that are logged while running a command get nicely displayed on the console. With _nicely_ I mean that the messages
are timestamped, colored depending on the log level and error logs are even written to the error ouput (`php://stderr`).
Now you don't need to conditionally handle the verbosity settings anymore and you can consistently use the logger instead of the OutputInterface in your commands and the services.
Your services simply use a logger as usual (injected with dependency injection) and are not coupled to the console.

In the standard edition of Symfony 2.4 this feature is now included by default with [PR #557](https://github.com/symfony/symfony-standard/pull/557). You can even customize
the behavior as in the following example. With the `verbosity_levels` option you can adapt the above mentioned mapping between verbosity and log level. In the given example it will
also show notices in normal verbosity mode (instead of warnings only). Additionally, it will only use messages logged with the custom *my_channel* channel and it changes the
display style via a custom formatter. See also the [documentation](http://symfony.com/doc/current/cookbook/logging/monolog.html) for more information.

    services:
        my_formatter:
            class: Symfony\Bridge\Monolog\Formatter\ConsoleFormatter
            arguments:
                - "[%%datetime%%] %%start_tag%%%%message%%%%end_tag%% (%%level_name%%) %%context%% %%extra%%\n"
    
    monolog:
        handlers:
            console:
                type:   console
                verbosity_levels:
                    VERBOSITY_NORMAL: NOTICE
                channels: my_channel
                formatter: my_formatter

I think this solves a common problem when working with console commands. For your information, a
[different approach using AOP](http://php-and-symfony.matthiasnoback.nl/2013/07/symfony2-rich-console-command-output-using-aop/) has been described by Matthias Noback. 
Now it's your turn to make your commands more user and developer friendly by exposing clear information on what is happening without having to resort to verbose or dirty code.
