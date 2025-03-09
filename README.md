From SmoothTerminal's video ["[Sidekiq 001.2] Installation and First Job - DailyDrip"](https://www.youtube.com/watch?v=bfPb1zD91Rg)

# Usage
First install the dependencies with `bundle`.
You'll need three separate terminals.

## Terminal 1: Redis
Start Redis, the easiest way to do this is using Docker:
```sh
docker run --network host redis
```

You could pass the `-d` flag if you want it to run in the background.

## Terminal 2: Sidekiq
Run our Sidekiq worker:
```sh
bundle exec sidekiq -r ./worker.rb
```

## Terminal 3: IRB
Run an IRB session to start jobs:
```sh
bundle exec irb -r ./worker.rb
```

From there you can enqueue jobs with
```rb
OurWorker.perform_async("easy")
```

This job will take one second to run and signal completion in the terminal running Sidekiq by printing to the console.
To run longer running jobs, pass `"hard"` (10 seconds) and `"super hard"` (20 seconds) instead of `"easy"`.

Note how the calls are non-blocking and you can queue multiple jobs without waiting for them to complete.

To schedule a job to run later you can use the `#perform_in` method:
```rb
OurWorker.perform_in(5, "easy")
```
This will wait five seconds before starting the job.
