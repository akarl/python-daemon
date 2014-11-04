# Simple python daemon.

## Todo

- create setup.py
- write tests

# Example

    class MyDaemon(Daemon):
        run_it = True

        def terminate(self):
            self.run_it = False

        def run(self):
            while self.run_it:
                with open('/tmp/time.test', 'a') as f:
                    f.write('start')

                for i in range(60):
                    with open('/tmp/time.test', 'a') as f:
                        f.write(str(datetime.datetime.now()) + '\n')
                    time.sleep(1)

                with open('/tmp/time.test', 'a') as f:
                    f.write('stop')


    if __name__ == "__main__":
        daemon = MyDaemon('/tmp/daemon-example.pid', stdout='/tmp/test.out', stderr='/tmp/test.err')

        if len(sys.argv) == 2:
            if 'start' == sys.argv[1]:
                daemon.start()
            elif 'stop' == sys.argv[1]:
                daemon.stop()
            elif 'restart' == sys.argv[1]:
                daemon.restart()
            else:
                print "Unknown command"
                sys.exit(2)
            sys.exit(0)
        else:
            print "usage: %s start|stop|restart" % sys.argv[0]
            sys.exit(2)

As found here http://www.jejik.com/articles/2007/02/a_simple_unix_linux_daemon_in_python/ with modifications.
