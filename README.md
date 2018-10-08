# Uber - Predictive Load Management

**Problem Statement:** 

Ride-sharing companies such as Uber and Lyft have historically had issues with large-scale events (inlduing concerts and sports events) to the extent that customers are forced to either accept high wait times or exorbitant surge priing. The below analysis attempts to suggest a solution to that problem, by predicting requests ahead of time, which would allow a company to pre-emptively relocate vehicles in anticipation of demand spikes. 

The project was part of IEOR 4418 - Transportation Analytics and Logistics at Columbia.

**Data Sources**

In order to correctly test the effectiveness of our approach, we select a set of control dates and a single effect date. Since the available [Uber data](https://github.com/fivethirtyeight/uber-tlc-foil-response) was from 2014, we picked Tuesday May 13th, 2014 to be the effect day: that night Lady Gaga performed at a sold-out Madison Square Garden. In order to eliminate any potential distortion due to day of week, we picked the remaining Tuesdays in May 2014 to be in the control group, making sure that no extr-ordinary events took place during those days. Given the limited data available from Uber, we used [NYC Yellow Cab data](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml) as a substitue to estimate several of our model parameters

**Methodology:**

- Step 1: manually create geolocation hashes using [geohash](http://geohash.gofreerange.com/), a tool that has a corresponding [Python API](https://pypi.org/project/pygeohash/) to easily turn the hashes into lat / lon pairs. Using these gehashes, we created three concentric zones surrounding Madison Square Garden, with area 1 being the area immediately surrunding MSG.

<p align="center">
<img src="Images/Circles.png" style="display: block; margin: auto;" height="300" width="375" />

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For the purposes of our modeling, we assumed area 3 to be all locations within a 15-minute driving radius of area 2. &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We never assume a customer in area 1 to be paired with a driver in area 3, but still included it in order to model the &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;process with an infinite queue model (i.e. all customers will be served)

- Step 2: Define problem: We decided to approach the problem from two different perspectives: a linear program and simluation. The linear program(s) were primarily used to find the optimal relocation of vehicles between the three areas, given the provided Uber trip data. We then used simulation to understand how robust our estimations were: given a Poisson process of customer arrivals, how much would profit increase as a function of demand, and what are break-even levels

- Step 3: Estimate parameters: due to the sparse data available from Uber, we relied on estimation techniques to obtain most of our model parameters
    - Actual (observed) supply of Ubers in area j, obtained from [Uber data](https://github.com/fivethirtyeight/uber-tlc-foil-response) : N<sub>0j</sub>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |   N_0j   | 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| - | -------- |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 1 | 5.3333   | 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 2 | 43.6667  |  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 3 | 704.6667 | 


x<sub>ij</sub>

|   |     1    |    2     |     3    |
| - | -------- | -------- | -------- |
| 1 | 0.992608 | 0.686474 | 0.172143 |
| 2 | 0.686474 | 0.992608 | 0.541465 |
| 3 | 0.172143 | 0.541465 | 0.992608 |

c<sub>ij</sub>

|   |    1    |    2    |    3    |
| - | ------- | ------- | ------- |
| 1 | 0       | 4.14604 | 8.19311 |
| 2 | 4.14604 | 0       | 5.24474 |
| 3 | 8.19311 | 5.24474 | 0       | 



r = 13.54
