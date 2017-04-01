---
layout:     post
title:      Nuit du Hack XV - Slumdog Millionaire
subtitle:   ""
date:       2017-04-01 00:00:00
author:     "Martin Ingesen"
header-img: "img/post-bg-nuitduhack.jpg"
permalink:  "writeups/nuit-du-hack-slumdog-millionaire"
---

### Description

The PiggyBank corporation has dediced to open source its new gambling application, SlumdogMillionaire ! Since then, it has become a world phenomenon. They are particularly proud of their new algorithm and hope to prove their game is fair by giving "Carte Blanche" for anyone to audit their code !

``algoritm.py``

    #!/usr/bin/python2.7

    import random

    import config
    import utils


    random.seed(utils.get_pid())
    ngames = 0


    def generate_combination():
        numbers = ""
        for _ in range(10):
            rand_num = random.randint(0, 99)
            if rand_num < 10:
                numbers += "0"
            numbers += str(rand_num)
            if _ != 9:
                numbers += "-"
        return numbers


    def reset_jackpot():
        random.seed(utils.get_pid())
        utils.set_jackpot(0)
        ngames = 0


    def draw(user_guess):
        ngames += 1
        if ngames > config.MAX_TRIES:
            reset_jackpot()
        winning_combination = generate_combination()
        if winning_combination == user_guess:
            utils.win()
            reset_jackpot()


### Solution

To solve this we need to know the next winning combination. We can see from the code that ``random`` is being seeded using ``utils.get_pid()``. This means that if we can figure out the pid, we have the seed, which mean we can generate the next winning combination ourself!

Solving this is therefore a matter of brute force. We simply iterate from 0 to 9999, hoping that the pid is going to be between those two numbers.
We set our own random seed using the iterator, then we generate a combination. If the combination is the same as the one from the website, we have found the pid! Then we can simply run the generator once more, and we will have the next winning combination. This looks something like this in python:

    #!/usr/bin/python2.7
    import sys
    import random

    if len(sys.argv) < 2:
        print "Give me the winning combination"
        sys.exit()

    last_combination = sys.argv[1]

    def generate_combination():
        numbers = ""
        for _ in range(10):
            rand_num = random.randint(0, 99)
            if rand_num < 10:
                numbers += "0"
            numbers += str(rand_num)
            if _ != 9:
                numbers += "-"
        return numbers

    for i in range(0, 9999):
        random.seed(i)

        if generate_combination() == last_combination:

            print "Found seed: " + str(i)
            print "Next winning combination is: " + generate_combination()

Using this on the challenge website, we get:

    YOU WON !
    Here is the code to claim your 20 NDHcoins: flag{God_does_not_pl4y_dic3}
