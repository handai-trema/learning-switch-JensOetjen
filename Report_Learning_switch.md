# Report: Accelerating Cbench #

Date: 24.10.2016 <br />
Name: Jens Oetjen <br />
Student Number: 33E16024 <br />

## 1 Introduction ##

To determine to the bottleneck of cbench, a profiler needs to be used that shows how much
time the program requires. The output by the profiler features how much time is spent in the functions.
By doing this, bottlenecks can be discovered. I used the profiler "ruby prof". After identifying
the bottleneck, I explain the code of the faster version of Cbench.

## 2 Using ruby-prof ##

I started two terminals. In the first terminal, I started ruby-prof with the follwing command 
which made it print its output into a file:

```
ruby-prof ./bin/trema r ./lib/cbench.rb &> output_of_ruby_prof_.txt
```

In the second Terminal, I used the following command to simulate traffic:

```
./bin/cbench --port 6653 --switches 1 --loops 10 --ms-per-test 10000 --delay 1000 --throughput
```

After the simulation was over, I stopped cbench in the first terminal with ctrl+c.

The resulting output-file can be found at:

[Output-File](https://github.com/JensOetjen/handai-trema-learning-switch-JensOetjen/blob/master/output_of_ruby_prof_.txt)

## 3 Explanation ##

The "fast version" is faster because it creates only a message when necessary (for example, the first time)
while the slow version creates a message every time. The following 
two lines of code from the fast version are responsible for this behaviour. A message is only created when @flow mod is null:


```
   @flow_mod ||= create_flow_mod_binary(packet_in)
    send_message dpid, @flow_mod
```

In contrast to that, the slow version creates a new message every time. Therefore it is much slower.
