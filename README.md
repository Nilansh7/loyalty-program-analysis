# Loyalty Program Analysis – October Dataset

This project presents a detailed analysis of user activity data from a loyalty-based gaming platform. The analysis involves calculating loyalty points, identifying top-performing users, allocating bonus rewards based on engagement and transactions, and evaluating the fairness of the existing loyalty point formula.

The project simulates a real-world case where a company wants to understand and reward its most loyal users through a data-driven approach.

---

## Project Objective

The primary objective of this analysis is to process player activity data for the month of October and achieve the following:

1. Calculate loyalty points for each user based on their deposits, withdrawals, and games played.
2. Rank users based on their total loyalty points across the entire month.
3. Allocate a bonus reward pool of ₹50,000 to the top 50 ranked players using a fair and balanced method.
4. Evaluate the fairness of the loyalty point calculation formula and suggest improvements to make it more robust and equitable.

This project not only demonstrates data cleaning and processing techniques using Python (primarily pandas), but also applies business logic to real-world scenarios involving user engagement and incentives.

---

## Dataset Overview

The dataset consists of three Excel sheets representing user activity data for the gaming platform. Each sheet captures different aspects of user interaction:

1. **Game Activity Sheet**: Records the games played by users, along with timestamps and user IDs.
2. **Deposit Sheet**: Contains information about the amount deposited by each user, along with timestamps and transaction counts.
3. **Withdrawal Sheet**: Contains withdrawal transactions with amounts, timestamps, and user IDs.

These datasets allow us to track financial contributions (deposits and withdrawals) as well as user engagement (games played), which are crucial for loyalty measurement.

---

## Time Slot Segmentation

Each day is divided into two time slots to analyze user behavior in more granular detail:

- **Slot S1**: 12:00 AM to 11:59 AM
- **Slot S2**: 12:00 PM to 11:59 PM

This segmentation enables the identification of peak activity times and user contribution patterns during different parts of the day. Specific slots such as 2nd October S1, 16th October S2, 18th October S1, and 26th October S2 are analyzed for sample validation.

---

## Part A – Loyalty Points Calculation

Loyalty points represent the total value a user brings to the platform through financial transactions and engagement. The given formula to calculate loyalty points is:

**Loyalty Points = 0.01 × Total Deposit Amount + 0.005 × Total Withdrawal Amount + 0.001 × (Deposit Count - Withdrawal Count) + 0.2 × Total Games Played** 


### Explanation of Formula Components:

- **Deposit Amount**: A user earns 1 loyalty point for every ₹100 deposited. This reflects their financial contribution to the platform.
- **Withdrawal Amount**: Users are rewarded 0.5 points for every ₹100 withdrawn. Though withdrawals are outflows, they still indicate user activity.
- **Transaction Frequency**: Additional points are awarded for higher deposit frequency relative to withdrawals, promoting consistent engagement.
- **Games Played**: A significant weight (0.2 per game) is given to active participation, ensuring that regular gameplay is appropriately rewarded.

This formula is applied across the entire month of October for each user, after aggregating their transactions and gameplay activity.

### Output:
The output of this section is a ranked list of users with their total loyalty points, sorted in descending order. This list is exported as a CSV file for reporting and used in the bonus allocation step.

---

## Part B – Bonus Distribution Strategy

The company has a fixed bonus pool of ₹50,000 to distribute among the top 50 ranked users. The challenge is to determine a fair way to divide the bonus such that it rewards users proportionally based on their value to the platform.

### Problem Consideration:

If the bonus is awarded solely based on loyalty points, it may favor users who deposit large amounts but do not actively play games. Conversely, if it is based only on games played, it may reward users who contribute less financially. Therefore, a more balanced approach is needed.

### Proposed Solution: Hybrid Scoring Model

To achieve fairness and inclusivity, a hybrid scoring model is introduced. This model calculates a final score for each user based on a weighted combination of loyalty points and games played.

#### Formula:

**Final Score = 0.7 × (Normalized Loyalty Points) + 0.3 × (Normalized Games Played)**


- Loyalty Points and Games Played are normalized using Min-Max scaling so that both features contribute comparably.
- A 70% weight is assigned to loyalty points, emphasizing financial engagement.
- A 30% weight is assigned to games played, acknowledging active user participation.

#### Bonus Allocation:

The total bonus pool is distributed based on the final score share:

**User Bonus = (Final Score / Sum of All Final Scores) × ₹50,000**


This approach ensures that high-value users (in terms of money and engagement) receive proportionate rewards, while also recognizing players who actively contribute to the platform’s daily activity.

### Output:
The result is a CSV file listing the top 50 users along with their loyalty points, games played, final score, and bonus amount.

---

## Part C – Evaluation of the Loyalty Formula

While the given formula is functional and straightforward, it is important to evaluate whether it truly reflects user loyalty in a fair and balanced manner.

### Observations:

1. **Overweight on Deposits**: The formula heavily favors users who deposit large amounts, regardless of whether they play games or not.
2. **Underweight on Engagement**: Playing multiple games contributes very little to the total score, which can discourage regular, low-spend users.
3. **Risk of Negative Scores**: If a user makes more withdrawals than deposits, the term `(Deposit Count - Withdrawal Count)` may be negative, reducing their overall score unfairly.
4. **No Reward for Consistency**: Users who are active over many days are not differentiated from users who perform all actions on a single day.

### Suggested Improvements:

To create a more robust and inclusive loyalty system, we propose the following changes:

#### Improved Formula:

**Improved Loyalty Points = 0.008 × Total Deposit Amount + 0.003 × Total Withdrawal Amount + 0.3 × Total Games Played + 2 × Number of Active Days + 0.001 × (Deposit Count - Withdrawal Count).clip(lower=0)**


#### Explanation of Changes:

- Reduced weight for deposits and withdrawals to reduce financial bias.
- Increased weight for games played to emphasize user engagement.
- Added a term for number of active days to reward consistency.
- Applied clipping to prevent negative scores from transaction imbalances.

This enhanced formula is more aligned with rewarding long-term active users, not just high spenders.

---

## Tools and Libraries

- Python 3.x
- pandas
- numpy
- openpyxl
- matplotlib (optional, for visualization)
- seaborn (optional)

Install required packages using:

## pip install -r requirements.txt

## Conclusion

This project demonstrates how data analysis can be used to support business decisions involving customer rewards and engagement. By building a fair and balanced scoring system, we ensure that both monetary contributions and active participation are recognized and rewarded.

The approach outlined here can be applied in various industries including gaming, fintech, and e-commerce, where user loyalty is critical to long-term success.

Feedback, contributions, and suggestions are welcome. If you found this project insightful, feel free to fork the repository or raise an issue with your suggestions.


