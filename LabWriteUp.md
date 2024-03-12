## Lab Report 5 - Putting it All Together (Week 9)
# Part 1 – Debugging Scenario

**EDSTEM POST:**

![image](LR5SS1.png)


**TA RESPONSE:**

![image](LR5SS2.png)

**FIXED CODE**

```
if grep -q "OK" TestOutput.txt
then 
    echo "You pass!"
    lastline=$(cat TestOutput.txt | tail -n 2 | head -n 1)
    tests=$(echo $lastline | awk -F'[()]' '{print $2}' | awk '{print $1}')
    echo "Your score is $tests / $tests"
else
    echo "You fail!"
    lastline=$(cat TestOutput.txt | tail -n 2 | head -n 1)
    tests=$(echo $lastline | awk -F'[, ]' '{print $3}')
    failures=$(echo $lastline | awk -F'[, ]' '{print $6}')
    successes=$((tests - failures))
    echo "Your score is $successes / $tests"
fi
```

From the TA response, I tried printing the lastline variable, which printed "OK (2 Tests)". From this, I realized that the bug only happens for passed tests, so I edited the if statement to the following structure: 

```
if (passes)
then
  print $tests / $tests
else
  print $successes / $tests
fi
```
I used the `awk` command to isolate the number of tests in lastline, and set that number to the variable tests. Because the code worked fine for failed tests, I left the remaining code in the if loop alone. I also removed the single quotes from the if condition so the argument is treated as a command rather than a string.


The bug was that for passed tests, instead of having several fields describing the failed tests, it just states the number of tests passed and the time it takes. Because of this, the `awk` command does not work as intended and is not able to assign values to the tests, failures, and successes variables. 


**Directory Structure**
![image](structure1.png)
![image](structure2.png)

The images above show the structures of the relevant directories: Lab7 and grading-area.


