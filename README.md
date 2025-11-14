# Jarkom-Modul-4-2025-K58

## CIDR Calculation

Link CIDR calculation: https://cryptpad.disroot.org/sheet/#/2/sheet/view/F8AG5XtoutJzq5eTlJp4b-WXt5N7Et7QBZmAiBHBpSk/

Things to do/watchout for:
1. Firstly, map the demanded host, net mask, net addr, usable ip range, and broadcast addr of every subnet to a spreadsheet.
2. Supernet it from either top/bottom, NOT MIDDLE.
3. Draw the supernet as a tree or whatever you fancy to help visualize.
4. Router subnets dont have to be combined with host subnet one by one, eg. here A1 -> A2 -> A3 -> Internet:

```
Host subnet: A1
Router subnet 1: A2
Router subnet 2: A3

Subnet goes like: B1 = A1, A2, A3

NOT: B1 = A1, A2; B2 = B1, A3
```

4. Careful with routing's prefix assignment and its matching subnet, eg. if B1 is 192.240.0.0/22, then B2 must be started from 192.240.4.0/22 or higher. This extends into supernet's calculation process where it must pay attention to prev supernet for same reason.

6. In a supernet, subnets goes contiguously (means addr of one subnet continues to other subnet like a chained link)

```
A1: 192.240.0.128/25
A2: 192.240.1.0/30
A3: 192.240.1.4/29
A4: 192.240.4.0/22

Not a good idea since there is a gap between A3-A4
```
