# Study Case: Hotel Booking Cancelation Prediction

### Notice:
The dataset used in this analysis is a dummy dataset. It doesn't contain actual, sensitive, and real business data.

### Context:
This hotel was established in July 2018, located in Lagos, Algarve Region, Portugal. The hotel is assigned as 4-star hotel.\
In year 2025, based on Government Statistics, Occupany Rate for the region was 61.4% for year 2025.\
<img width="885" height="327" alt="image" src="https://github.com/user-attachments/assets/e0885d88-e521-469f-a6af-8fa33f0f599b" />
Source: https://thehotelblueprint.com/hotel-market-intel/hotel-performance/portugal-full-year-2025/ \
The hotel accepts thousands of reservations each year. Within those reservations, there are a lot of cancelation from the hotel guests. This will affect hotel's daily occupancy rate, as the room can not be sold for the day because hotel do not receive any confirmation before D-day. General Manager (stakeholder) decide to find a way on how to predict customer (hotel guest) bookings, which are likely to be canceled.

**Prediction outcome:**
* Guests who will cancel their bookings
* Guests who will not cancel their bookings
To predict the outcome, Machine Learning Classification algorithm will be used.

### Metrics:
| Condition | Impact |
| -- | -- |
| False Positive | Guests that are falsely predicted to cancel their hotel bookings.<br> If we do not accept these reservations, we will suffer from loss in revenue and greater loss in trust from the hotel guests. There is a possibility this guest will not make future bookings anymore. |
| False Negative | Guests that are falsely predicted to stay, but they cancel their hotel bookings or no-show on that day.<br>All the prepared-rooms will become vacant. It can be sold to potential walk-in customer, only if they cancel their booking. This will cause low occupany rate of the day, and in longer terms will affect future potential investors decision, whether they should invest or not in a hotel with 'low occupancy rate'. |

Daily occupany rate is calculated based on **the actual number of rooms** occupied by guests. Even the guests booked the room, but do not stay for the night, it will not be counted; lowering the occupany rate.
<center><b>Occupancy Rate = (Number of Occupied Units / Total Number of Available Units) x 100%</b></center>

<br>Choosing a best metric to evaluate the model, will depend on the hotel's business model.\
If the hotel is affiliated with a hotel chain program (e.g. IHG, Accor, Conrad), they will value "trust" from the hotel guests the most. Hence, the model should focus on lowering the 'False Positive' rate.\
If the hotel is non-affiliated and have high daily occupany rate most of the time, they will need a model who can predict guests that will likely to cancel their bookings. So, they can resell the room to another guests. With this business model, the hotel will focus on lowering the 'False Negative' rate. Furthermore, this is also applied if the hotel's owner want to attract new investors.

Each model designed specifically to each business model.\
Considering the dataset is not provided with "Date" column and daily occupancy rate is unknown,\
this model will focus on predicting "Who are the hotel guests that will likely to cancel their bookings" with **Low False Positive Rate**.\
Therefore, the F-score (beta = 0.5) will be used.

Another thing to consider, we want our model to predict hotel guests who will cancel. We do not want to falsely predict "True Staying hotel guest". If we falsely predict (False Positive), this will create another issue. We will lose their trust, and there's possibility they will not book again in the future (Churn). 

### Data Understanding:
**Features:**\
*All personally identifying information has been removed from the data.*

| Features | Description |
| -- | -- |
| country| Country of guests origin. |
| market_segment | Market segment designation. |
| previous_cancellations | Number of previous bookings that were cancelled by the customer prior to the current booking. |
| booking_changes | Number of changes/amendments made to the booking from the moment the booking was entered on the PMS until the moment of check-in or cancellation. |
| deposit_type | Indication on if the customer made a deposit to guarantee the booking. |
| days_in_waiting_list | Number of days the booking was in the waiting list before it was confirmed to the customer. |
| customer_type | Type of booking. |
| reserved_room_type | Code of room type reserved. Code is presented instead of designation for anonymity reasons. |
| required_car_parking_space | Number of car parking spaces required by the customer. |
| total_of_special_request | Number of special requests made by the customer (e.g. twin bed or high floor). |
| is_canceled | Value indicating if the booking was canceled (1) or not (0). |

