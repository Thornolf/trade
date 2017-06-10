#!/usr/bin/env python

import sys, os
from math import *

# ------------ ** MATH FUNCTION ** --------------- #
# computeEma is use to get the exponential moving average
# It take the previous EMA, the actual share and the actual day
# It return the EMA value

def computeEma(ema, share, day):
    value = 2.0 / (day + 1.0)
    out  = (value * share + (1.0 - value) * ema)
    return out


# ------------ ** UTILS FUNCTION ** --------------- #
# buyShare allow us to buy share and print "buy" and
# the number of share we want to buy
# It takes our wallet & actual share

def buyShare(wallet, share):
    shareBought = floor((wallet['capital']) / share)
    percentage = share * shareBought + ceil(0.15 / 100 * (share * shareBought))
    if wallet['capital'] - percentage < 0:
        shareBought -= 1
    if shareBought > 0:
        wallet['capital'] = wallet['capital'] - share * shareBought - ceil((0.15 / 100.0 * (share * shareBought)))
        wallet['share'] = wallet['share'] + shareBought
        printBuy(shareBought)
    else:
        printWait()

# sellShare allow us to buy share and print "sell" and
# the number of share we want to sell
# It takes our wallet & actual share
# If we can sell, we sell everything

def sellShare(wallet, share):
    if wallet['share'] > 0:
        printSell(wallet['share'])
        wallet['capital'] = wallet['capital'] + wallet['share'] * share - ceil((0.15 / 100.0 * (wallet['share'] * share)))
        wallet['share'] = 0
    else:
        printWait()

#buyOrSell check the EMA, if the curve goes up we sell,
# if it goes down we buy

def buyOrSell(intelEma, wallet, share):
    if intelEma['50'] < intelEma['150']:
        sellShare(wallet, share)
    elif intelEma['50'] > intelEma['150']:
        buyShare(wallet, share)

# ------------ ** CORE FUNCTION ** --------------- #
# coreFunction handle the while loop and initiliaze the currentDay,
# Setup the EMA with 50 days and 150 days that why in short run
# we don't earn a lot

def algoTrading(wallet, nbrDay):
    currentDay = 1
    currentShare = input()
    intelEma = {}
    intelEma['50'] = 0.0
    intelEma['150'] = 0.0
    while currentShare.upper() != "--END--":
        try:
            shareValue = int(currentShare)
        except (ValueError):
            print("Error: \'", currentShare, "\' must be an unsigned integer.", sep='')
            currentShare = input()
            continue
        if currentDay == nbrDay and wallet['share'] > 0:
            sellShare(wallet, shareValue)
        else:
            intelEma['50'] = computeEma(intelEma['50'], shareValue, 50)
            intelEma['150'] = computeEma(intelEma['150'], shareValue, 150)
            buyOrSell(intelEma, wallet, shareValue)
        currentDay += 1
        currentShare = input()

# ------------ ** PARSE FUNCTION ** --------------- #
# Simple function to check if everything given in param is valid

def parseInput(str):
    actualValue = 0
    try:
        actualValue = int(str)
        if actualValue < 0:
            raise ValueError("Negative Number")
    except (ValueError):
        print("Error : \'", str, "\' must be an unsigned integer.", sep='')
        sys.exit(84)
    return actualValue

# ------------ ** PRINT FUNCTION ** --------------- #
# Simple function which print sell, buy or wait with arguemnts

def printWait():
    print("wait\n", end='')

def printBuy(share):
    print("buy {}".format(share))

def printSell(share):
    print("sell {}".format(share))


# ------------ ** MAIN FUNCTION ** --------------- #
# Main function where we initialize all functions

def main():
    share = 0
    capital = parseInput(input())
    nbrDay = parseInput(input())
    wallet = {}
    wallet['capital'] = capital
    wallet['share'] = share
    algoTrading(wallet, nbrDay)



if __name__ == "__main__":
    main()
