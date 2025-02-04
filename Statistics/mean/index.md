---
author: "UmmIt"
title: "Mean Calculations for the weird Tech Nerd who struggles with Math"
description: "My perspective on calculating the mean in a way that makes sense to those who tech nerds and find math difficult ..."
tags: ["math", "programming", "statistics", "mean", "average"]
date: 2025-02-05T01:40:02+0800
thumbnail: https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Standard_Normal_Distribution.png/1280px-Standard_Normal_Distribution.png
---

## Stroy

I really struggle with math, but this semester I have to take a statistics course, and I have to admit that I’m finding it challenging. However, as someone with a programming background and a GNU/Linux user, I know how to code. So, this article—and more upcoming statistics articles—will use my perspective as a programmer to explain statistical concepts in a way that makes sense to those of us who find math difficult. This article specifically focuses on calculating the mean and is aimed at people like me those who are struggling with math but are also nerds of Linux and programming.

I often get lost in math, but I discovered a way of thinking about mean calculation that resonates with me. So, whether you’re a typical person, or math guy you definitely will say this guy really weird XD.

## What is the Mean?

As a programmer, you might find yourself dealing with data frequently. One common task is calculating the average of a set of numbers, often referred to as the `mean`. If you’re not comfortable with math.

In programming terms, the mean is simply a way to find the `central` value of a collection of numbers. Think of it as a way to summarize a list of values into a single representative value. 

Imagine you have an array (or list) of numbers, which we can think of as a collection of variables. For example:

```cpp
int numbers[] = {10, 20, 30, 40, 50};
```

Here, `numbers` is an array containing five values. The mean will give us a single number that represents the average of these values.

### How Do We Calculate the Mean?

To calculate the mean, we can follow these steps:

1. **Sum Up All the Values**: We need to add together all the numbers in our array. This is like creating a new variable that holds the total of all the values.

2. **Count the Number of Values**: We need to know how many values are in our array. This is like checking the size of the array.

3. **Divide the Total by the Count**: Finally, we take the total sum and divide it by the number of values. This gives us the mean.

Let’s see how this looks in code:

```cpp
#include <iostream>
#include <vector>

/* 
 * This function calculates the mean (average) of a vector of double values.
 * It sums all the values in the vector and divides by the number of elements.
 * 
 * Example:
 * Given the vector A = {5, 10, 15, 20, 25} with 5 elements,
 * The sum is: 5 + 10 + 15 + 20 + 25 = 75
 * The mean is: 75 / 5 = 15
 *
 * Note: If the input vector is empty, the function will return 0.0.
 */

double calculateMean(const std::vector<double>& data) {
    double sum = 0.0;
    for (double value : data) {
        sum += value;
    }
    return sum / data.size();
}

int main() {
    std::vector<double> data = {5.0, 10.0, 15.0, 20.0, 25.0};
    double mean = calculateMean(data);
    std::cout << "The mean is: " << mean << std::endl;
    return 0;
}
```

And the code is the explanation of the steps we mentioned above, As my view point, that clean and simple code that makes sense to me.
