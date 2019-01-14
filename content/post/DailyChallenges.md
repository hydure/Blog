---
title: "Daily Challenges"
date: 2019-01-13T19:09:08-05:00
draft: false
toc: false
comments: false
categories:
- Programming
tags:
- Daily Challenges
---

There is a subreddit, r/dailyprogrammer, that has weekly challenges of various difficulties that I will solve and discuss in posts from time to time.
<!--more-->

Sorry for not posting for a little over a month, I was caught up visiting friends and family during the holidays and getting back into the groove of things.

I first started with the latest challenge, [Challenge #371 N Queens Validator](https://www.reddit.com/r/dailyprogrammer/comments/ab9mn7/20181231_challenge_371_easy_n_queens_validator/), which was a fun warm-up. The following are my challenge and bonus answers:

    def qcheck(array):

        # Check for queens on the same row.
        for row in array:
            if array.count(row) > 1:
                return False
        
        for i in range(len(array) - 1):
            for j in range(i + 1, len(array)):
                # Check for queens on the same diagonals 
                # (either ascending or descending slopes).
                if  (abs(array[i] - array[j]) == abs(i - j)):
                    return False
        
        return True

    def qfix(array):

        # Check to see if the array needs to be fixed.
        if qcheck(array):
            return array
        
        for i in range(len(array) - 1):
            for j in range(i+1, len(array)):
                array[i], array[j] = array[j], array[i]
                if (qcheck(array)):
                    return array
                else:
                    array[i], array[j] = array[j], array[i]
        return []

I checked my code with the input provided by the challenge:

    if  ((qcheck([ 4, 2, 7, 3, 6, 8, 5, 1 ]) == True) and
        (qcheck([ 2, 5, 7, 4, 1, 8, 6, 3 ]) == True) and
        (qcheck([ 5, 3, 1, 4, 2, 8, 6, 3 ]) == False) and
        (qcheck([ 5, 8, 2, 4, 7, 1, 3, 6 ]) == False) and
        (qcheck([ 4, 3, 1, 8, 1, 3, 5, 2 ]) == False)):
        print ("Challenge completed!")

    if  ((qfix([ 8, 6, 4, 2, 7, 1, 3, 5 ]) == [4, 6, 8, 2, 7, 1, 3, 5]) and
        (qfix([ 8, 5, 1, 3, 6, 2, 7, 4 ]) == [8, 4, 1, 3, 6, 2, 7, 5]) and
        (qfix([ 4, 6, 8, 3, 1, 2, 5, 7 ]) == [4, 6, 8, 3, 1, 7, 5, 2]) and
        (qfix([ 7, 1, 3, 6, 8, 5, 2, 4 ]) == [7, 3, 1, 6, 8, 5, 2, 4])):
        print ("Bonus completed!")

And everything worked as intended.

I learned a Python list as a `count(arg)` method that counts how many instances of `arg` there are in the list, which can prove to be useful later on.

Having some more free time, I completed [Challenge #370 UPC Check Digits](https://www.reddit.com/r/dailyprogrammer/comments/a72sdj/20181217_challenge_370_easy_upc_check_digits/) which sadly had no bonus challenge to attempt.

        def upc(code):

            appropriateInputFlag = False
            
            if isinstance(code, int):
                code = str(code)
    
            if isinstance(code, str):
                appropriateInputFlag = True

                # Add any necessary leading zeros.
                code = code.zfill(11)
                
                # Calculate the remainder.
                M = (sum([int(i) for i in code[::2]]) * 3 + \
                     sum([int(i) for i in code[1::2]])) % 10
            
            if appropriateInputFlag:
                if M == 0:
                    return M
                else:
                    return 10 - M
            
            else:
                print("Inappropriate input.")

The creator of this challenge said that we could assume the input is either a string or an integer, so I created a function that accepted both. Testing showed that this implementation worked fine:

        if  ((upc(4210000526) == 4) and
            (upc(3600029145) == 2) and
            (upc(12345678910) == 4) and
            (upc(1234567) == 0) and
            (upc("4210000526") == 4) and
            (upc("3600029145") == 2) and
            (upc("12345678910") == 4) and
            (upc("1234567") == 0)):
            print("Challenge completed!")

I learned of the `isinstance()` Python function and [why calling it is the correct way to determine a varible's type in Python](https://www.geeksforgeeks.org/type-isinstance-python/) and I became a little more familiar with list comprehension, which is always fun to implement. I learned what the [function `zfill()` is](https://python-reference.readthedocs.io/en/latest/docs/str/zfill.html) as well which may prove to be useful again one day.

Thank you for reading, I'm sorry I have not been updating my blog recently: I have attempted a few more challenges and have been working on a side project that I will be uploading soon! And I will get back to completing Bandit, as well as attempting a few CTF challenges.