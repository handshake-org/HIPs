# HIP-0000 : Increase renewal fee to improve the tokenomics of Handshake.

```
Number:  HIP-0000
Title:   Increase and burn the renewal fee
Type:    Informational
Status:  Draft
Authors: James Mao <iamjamesmao@mailo.com>
Created: 2023-11-25
```

## Abstract

The Handshake is the great web3.0 basic facilities, but the price of the HNS is too low to prompt the development of The Handshake. 
A important reason is that  HNS has not capture the value of renewal domain. It is necessary to charge and burn the renewal fee.

## Motivation

Starting from the launch of the main network on February 17, 2020, as of November 29, 2023, for a total of 1375 days, Handshake has 12million domains and burned a total of 33million HNS. On average, it burns 24,000 HNS per day; In the past year, (33.3-29.1=4.2 million) burned an average of 11,500 HNS per day; Meanwhile, 144,000 new HNS are generated daily for miners. Therefore, the recent net inflation rate is 133,000 per day, and the current annual inflation rate is approximately 8.5%; The current total market value of Handshake is $12,362,796, and the total number of HNS is 618,139,800; The current HNS unit price is $0.02, far below the issuance price of $0.10340. 

## Body

The Handshake domain name has enormous value, but HNS performs poorly because HNS has not captured its rightful value; The average price of each Handshake domain is less than 3HNS, and at the current price of $0.06, it is still permanently valid This leads to a huge vulnerability in HNS, where HNS loses its rightful value - domain ownership value, which is the renewal fee Currently, renewal is free, which means that holding HNS domain names does not require any cost, only requiring free renewal every 2 years and paying some negligible network fees This is a huge loss for HNS, causing great harm to HNS. The harm is intuitively seen as the unit price of HNS being too low, the overall market value of Handshake being too low, even lower than many worthless air coins. This low unit price causes losses for investors and miners, leading to their gradual departure, reduced investment in mining equipment and electricity, and ultimately reduced computing power to maintain network operation, It poses a threat to the survival and development of Handshake in the future, which is not conducive to its long-term development.


In order to change the above situation, it is necessary to charge for domain name ownership at a suitable time, that is, to charge renewal fees The fee for a renewal can be between 1-10 HNS After the renewal fee is collected, it immediately burns up and injects this value into the entire Handshake network This is the simplest and most feasible way to increase HNS value capture . 

After being charged and burned, it is estimated that 6 million (1HNS per renewal) to 60 million HNS will be burned annually (10HNS per renewal), with an annual inflation rate of 7.3% (1HNS per renewal) or 0% (10HNS per renewal); Referring to other systems such as ENS and. bit domain names, the holding fee is approximately $5 per year and $10 per 2 years HNS renews every two years If the HNS unit price is less than $1, the renewal fee can be 10HNS. If the HNS unit price is $10, the renewal fee can be 1HNS.



I agree that "The challenge with this is the price. 1 HNS may be $100 one year, and 10 HNS may be worth $0.50 the next year. There would need to be some mechanism for setting the fee. Currently that mechanism is the block size limit, which introduces a bidding market for transaction fees."( Mr. Pinheadmz)

Yes, it's difficult to give an absolutely accurate price, as HNS prices fluctuate; Through observation, we have found a high correlation between block difficulty and price. Therefore, the following is a pricing mechanism for reference:


1. Pricing goal: To achieve zero inflation or slight deflation in HNS

There are 5 factors to consider in pricing, including block difficulty, total number of domain names, number of block rewards, and block size Total market value

3. Pricing is automatically adjusted and calculated every certain time or block


Represented by formula:

Fee=f1 (block difficulty) * f2 (total number of domain names) * f3 (number of block rewards, block size, total market value)


Among them, f3 can be simplified as:

F3 (number of block rewards, block size, total market value) ~= number of block rewards/100;


After the first halving, f3=10HSN;

After the second halving, f3=5HNS;

After the third halving, f3=2.5HNS


Calculation of f1:

△1=Log(Difficulty1) - log(Difficulty0);

If (△1>0): f1=1/(1+△ 1);
If (△1<0): f1=1;

Difficulty0 is the block difficulty before the previous time or block interval, and Difficulty1 is the block difficulty at the current calculation time;

Calculation of f2:

△2=Log(TotalDomains1) - log(TotalDomains0);

If (△2>0): f2=1;
If (△1<0): f2=1/(1+△2);

Total Domains0 is the total number of domain names before the last time or block interval, and Total Domains1 is the total number of domain names during the current calculation;

Finally, the calculation of renewal fee:

Fee=f1 * f2 * f3



## References

1. https://e.hnsfans.com
2. https://theshake.xyz
