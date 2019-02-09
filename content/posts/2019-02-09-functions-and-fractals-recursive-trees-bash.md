---
template: post
title: Functions and Fractals - Recursive Trees - Bash!
slug: /posts/bash-tree
draft: false
date: 2019-01-11T11:26:28.627Z
description: A solution of Hackerrank question about printing functions and fractals!
category: Code Challenge
tags:
  - code
  - bash
  - challenge
---
This is a solution for [Functions and Fractals - Recursive Trees - Bash!](https://www.hackerrank.com/challenges/fractal-trees-all/problem)

The challenge involves the construction of trees, in the form of ASCII Art with using bash.

![](/media/fractal-bash.png)

I used the following code to solve this question.

```bash
#!/bin/bash
declare -A matrix
num_rows=63
num_columns=100
read depth

for ((i=1;i<=num_rows;i++)) do
    for ((j=1;j<=num_columns;j++)) do
        matrix[$i,$j]="_"
    done
done

set_position () {
    posX=$1
    posY=$2
    direction=$3
    length=$4
    if [ "$direction" == "U" ]
    then
        for ((i=0;i<length;i++)) do
            matrix[$((posX-i)),$posY]="1"
        done
    elif [ "$direction" == "R" ]
    then
        for ((i=0;i<length;i++)) do
            matrix[$((posX-i)),$((posY+i))]="1"
        done
    else
        for ((i=0;i<length;i++)) do
            matrix[$((posX-i)),$((posY-i))]="1"
        done
    fi
}

printMatrix() {
    for ((i=1;i<=num_rows;i++)) do
        for ((j=1;j<=num_columns;j++)) do
            printf ${matrix[$i,$j]}
        done
        echo
    done
}

draw_tree (){
    local x=$1
    local y=$2
    local len=$3
    set_position $x $y U $len
    set_position $x-$len+1 $y L $len+1
    set_position $x-$len+1 $y R $len+1
}

coef=1
first=0
for ((l=1;l<=depth;l++)) do
    for ((j=0;j<coef;j++)) do
        v1=$(( 64 / $coef - 1 ))
        v2=$(( 50 - $first + $j * 64 / $coef))
        v3=$(( 16 / $coef))
        draw_tree $v1 $v2 $v3
    done
    ((coef=2*$coef))
    ((first+=32/$coef))
done

printMatrix
```
