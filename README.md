# 📡 Satellite Pass Scheduling Optimization

## 📖 Project Description

This project implements several approaches to solve a satellite pass scheduling optimization problem.  

The objective is to:

- Select satellite passes  
- Define booking time windows  
- Satisfy observation requests  
- Minimize the total operational cost  

The project includes:

- A simple heuristic approach  
- Several optimization models using DOcplex  
- Geographic visualization of solutions  
- Cost vs. satisfaction trade-off analysis  

---

## ⚠️ Non-Public Data Files

The following files are **NOT public** and are **not included in this repository**:

- `dataX.json`
- `solutionX.json`

These files contain:

- Satellite passes  
- Access windows  
- Areas of Interest (AOIs)  
- Observation requests  
- Cost parameters  
- Generated solutions  

To run the project, you must provide your own data files following the same structure.

---

## 📦 Installation

Install required dependencies:

```bash
pip install cartopy
pip install matplotlib
pip install numpy
pip install docplex
```

⚠️ Note: DOcplex requires IBM CPLEX to be properly installed and configured.

---

## 📂 Expected Data Structure

The `dataX.json` file must contain:

- `AOIs` — Areas of Interest (latitude, longitude)
- `Accesses` — Access windows linked to satellite passes
- `Passes` — Satellite passes (`start`, `end`)
- `ObservationRequests` — Requests with priorities and possible accesses
- `fixedPassCost`
- `passCostPerTimeUnit`
- `minVisiDuration`

Solutions are stored in the following format:

```json
{
  "Bookings": [
    {
      "passId": 0,
      "passStart": ...,
      "passEnd": ...,
      "bookingStart": ...,
      "bookingEnd": ...
    }
  ]
}
```

---

## 🧠 Implemented Methods

### 1️⃣ Random Heuristic

- Random selection of satellite passes  
- Covers observation requests when possible  
- Exports solution to `mySolution.json`  

---

### 2️⃣ DOcplex Model – Full Coverage

Objective:

Minimize total cost:

$$
\sum (fixed\ cost + duration \times unit\ cost)
$$

Decision variables:

- `p[i]` — binary variable indicating whether pass *i* is selected  
- `x[i]` — binary variable indicating whether request *i* is satisfied  

Constraints:

- Each request must be covered by at least one selected pass  
- All requests must be satisfied  

---

### 3️⃣ Advanced Model – Booking Window Optimization

Adds:

- Continuous variables `date_start[i]`
- Continuous variables `date_end[i]`
- Binary variables `acces_couvert`
- Minimum booking duration constraint (30 seconds)
- Big-M constraints to enforce time feasibility  

This model optimizes both pass selection and booking time intervals.

---

### 4️⃣ Cost vs Satisfaction Trade-off (Step 4)

- Introduces parameter `alpha` (minimum satisfaction ratio)
- Optional priority weighting using exponent `w`
- Solves the model for multiple values of `alpha`
- Produces a cost vs. satisfaction scatter plot  

This allows analysis of the Pareto trade-off between cost and service level.

---

## 📊 Utility Functions

### `cout(data, solution)`

Computes total cost:

$$
cost = \sum (fixed\ cost + duration \times unit\ cost)
$$

---

### `satisfaction(data, solution)`

Computes:

$$
\frac{\text{satisfied requests}}{\text{total requests}} \times 100
$$

---

### `showSolution(data, solution)`

Displays selected passes on a geographic map using Cartopy:

- AOIs shown as black crosses  
- Covered accesses in red  
- Non-covered accesses in blue  

---

## ▶️ Running the Project

1. Place your `dataX.json` file inside a `data/` folder  
2. Run the Python script  
3. The solution will be exported to:

```
mySolution.json
```

4. The program outputs:
   - Selected passes  
   - Total cost  
   - Satisfaction rate  
   - Optional map visualization  
   - Optional cost/satisfaction scatter plot  

---

## 🛠 Technologies Used

- Python 3  
- DOcplex (IBM CPLEX)  
- NumPy  
- Matplotlib  
- Cartopy  

---

## 🎯 Educational Purpose

This project demonstrates:

- Mixed Integer Linear Programming (MILP) modeling  
- Big-M constraint formulation  
- Multi-objective optimization (cost vs satisfaction)  
- Trade-off and Pareto frontier analysis  
- Practical satellite scheduling optimization  

---

## 📌 Notes

- The minimum observation duration is set to 30 seconds in the advanced models.
- A time limit is imposed on some optimization runs.
- The visualization part can be disabled if Cartopy is not properly installed.

---
