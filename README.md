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

### Data Preprocessing:
Handling missing values\
Handling outlier data\
Binning to simplify categorical data (by using Statistical test)\
Scaling to simplify numerical data\
Encoding categorical data (by using One-hot Encoder)

### Modeling Process:
Find best machine learning algorithm through cross validation\
First hyperparameter tuning through GridSearch\
Review model performance to validation data\
Review and reselect feature through feature importance\
Remodel dan hyperparameter tuning\
Find best model\
Second hyperparameter tuning through GridSearch\
Review model performance through Confusion Matrix

### Chosen Model Algorithm
Light Gradient Boost Classifier works well with F-score (beta = 0.5) on Test Data 76.02%.\
Features that are used a lot during tree split are: Booking Changes, Months Wait List, Customer Type (Transient & Transient Party) and European guests.
<img width="4154" height="2494" alt="Mod_LGBoost_Hotel_Booking_F_Importance" src="https://github.com/user-attachments/assets/c122ff93-6cd2-4f5c-bede-faca335ad5d6" />

### Model Interpretation
Model is interpreted through SHAP Beeswarm plot. 
<img width="2373" height="2819" alt="Mod_LGBoost_Hotel_Booking_SHAP_Plot" src="https://github.com/user-attachments/assets/9d1554a5-c4d9-46dd-93ee-be8620ead31f" />

**Guest who will likely cancel bookings**
* Non refund guests
* Domestic guests
* Online Travel Agent
* Without Request Parking
* Without Special Requests
* Guests without cancel history and 1 cancel history

### Business Impact:
| Before Machine Learning | After Machine Learning |
| --- | --- |
| <img width="455" height="330" alt="image" src="https://github.com/user-attachments/assets/7075311f-63b6-48d9-8cad-a688c5475c0c" /> | <img width="444" height="330" alt="image" src="https://github.com/user-attachments/assets/269bef3c-d04b-4aa5-80ab-b059cfce4216" /> |
|  | Threshold = 0.9 | 
| Canceled bookings : 6.156 bookings (from Test Data) <br> = 6.156 bookings x EUR 21 (EUR 70 x 30%) <br> = EUR 129k | Canceled bookings : 3.862 bookings (from Test Data) <br> Amount received from canceled bookings: <br> = 3.862 bookings x EUR 21 (EUR 70 x 30%) <br> = EUR 81k <br> Amount received from resell rooms: <br> = 2.294 bookings x EUR 70 x 50% rooms <br> = EUR 80k |
|  | Assumption: 50% of canceled rooms can be resell again for the same night |

**Saved Amount = 161k – 129k = 32k / year**\
This amount can be higher, if management/stakeholder decide to lower down threshold (as more True Positive predicted)

### Recommendations:
Customer who booked with 'No Refund Deposit' canceled their bookings mostly (99%).\
This issue can be solved by imply maximum check-in time for those bookings (Example: until 19:00 or early notice).\
After hotel guest do not show up, this room can be sold to another walk-in guests. Therefore, daily occupancy rate can be increased.\
Data from marketing segment shown that "Group" segment has 61% of canceled bookings. This group consist of 42% transient and 53% transient-party.\
Marketing can make special "hotel deals" targeted to this group.

### Model Improvement:
To improve this model to suit better for business decision and recommendation, it is essential to know about the daily occupancy rate.\
After knowing about this, it can be compared with the average occupancy rate of the city/country. Most of 4-star hotel recorded 70-80% daily occupancy rate in general. The number will be higher during high-season months, or lower during off-season. Therefore, Staying_Date date will be needed.\
Additionally, Staying_Month will also increasing model performance, because we can understand if there are specific seasonal bookings pattern.\
Example: hotel guests will not cancel their bookings during high-season month, and vice-versa. This can be combined with National Holidays or School Holidays too.\
If the hotel has membership, Membership_Level or Membership_Status also can be used to understand the customers behavior, and used as a feature to predict.









