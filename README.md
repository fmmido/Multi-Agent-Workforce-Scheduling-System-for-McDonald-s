# Multi-Agent-Workforce-Scheduling-System-for-McDonald-s
Multi-Agent Workforce Scheduling System for McDonald's

# YEPAI Multi-Agent Workforce Scheduling System for McDonald's

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![PuLP](https://img.shields.io/badge/PuLP-optimization-orange)](https://coin-or.github.io/pulp/)

A **multi-agent AI-powered workforce scheduling prototype** built for the YEPAI Hackathon challenge: replacing manual restaurant rostering with an intelligent, constraint-aware algorithmic manager.

This system generates a complete, conflict-minimized 2-week roster for a McDonald's CBD store (40 crew + 6 managers) in under 180 seconds while respecting Australian Fair Work Act rules, employee availability, skill matching, peak demand, and management presence.

## 🚀 Features

- **Multi-Agent Architecture** with clear roles (Demand Forecast, Skill Matching, Scheduling Optimizer, Conflict Resolution, Compliance Verifier)
- **Constraint Satisfaction Problem (CSP)** modeling using PuLP linear programming
- **Hard constraints enforced**: 
  - Australian labor laws (10h rest, max 6 consecutive days, weekly hour limits)
  - Skill-station matching (Kitchen, Counter, McCafe, Dessert)
  - Minimum 1 manager on duty at all times
  - Opening/closing and peak period coverage
- **Soft constraints optimized**: fair hour distribution, minimum hours fulfillment
- **Automatic conflict detection & resolution**
- Generates clean Excel output with Schedule, Hours Summary, and Conflicts sheets
- Runs in Google Colab (one-click execution)

## 📊 Project Structure

```
.
├── McDonalds_YEPAI_FINAL_WINNER_Roster.ipynb     # Main executable notebook
├── McDonalds_YEPAI_FINAL_WINNER_Roster     # Sample generated output (after run)
├── employee_availability_2weeks              # Crew availability & skills
├── management_roster_simplified              # Manager roster & availability
├── store_structure_staff_estimate.csv             # Demand per station (Normal/Peak)
├── australian_restaurant_rostering_parameters # Fair Work & params
├── fixed_hours_template_2columns              # Fixed task guidelines
├── store_configurations                       # Store type config
└── README.md                                      # This file
```

## 🏗️ Multi-Agent Architecture

| Agent                        | Role                                                                 | Key Actions |
|-----------------------------|----------------------------------------------------------------------|-----------|
| **DemandForecastAgent**     | Calculates staffing needs per period/day (incl. +20% weekends)       | Populates shared demand memory |
| **AvailabilityContractAgent**| Validates employee types, contracts, availability                    | Basic sanity check |
| **SkillMatchingAgent**      | Ensures only qualified staff assigned to stations                    | Compatibility lookup |
| **SchedulingOptimizerAgent**| Core CSP solver using PuLP; day-by-day optimization                   | Primary assignment |
| **ConflictResolutionAgent** | Fixes coverage gaps and under-hour employees                         | Greedy flexible assignments |
| **ComplianceVerifierAgent** | Final guardrail: checks weekly hour limits; logs violations          | Safety & reliability |

Agents share state via a **blackboard pattern** (`memory` dict) — simple, effective coordination for hackathon scope.

## ⚙️ How It Works

1. **Load & Parse** all provided data files (crew, management, demand, parameters)
2. **Model shifts** (1F, 2F, 3F, S, SC, M) with start/end times and period coverage
3. **DemandForecastAgent** computes required staff per station/period
4. **SchedulingOptimizerAgent** builds and solves a PuLP model per day:
   - Maximizes assignments under hard constraints
   - Falls back to greedy if no optimal solution
5. **ConflictResolutionAgent** fills remaining gaps and ensures minimum weekly hours
6. **ComplianceVerifierAgent** validates final hours; re-runs resolution if needed
7. Export full roster + summaries to Excel


## 📈 Sample Output

- **Schedule sheet**: Day → Period → Employee → Shift → Station → Hours
- **Hours Summary**: Total hours per employee over 2 weeks
- **Conflicts sheet**: Any remaining violations (target: empty)

## 🛠 Known Limitations & Future Improvements

- Management loading is sensitive to Excel formatting (fixed in latest version)
- No explicit penalty rate minimization (soft constraint)
- Fixed tasks (cleaning, inventory) not yet allocated as dedicated shifts
- Sequential agent execution (could be enhanced with negotiation loops)
- No UI/dashboard (focus on backend intelligence)

**Winning Potential Upgrades** (for production/post-hackathon):
- True agent negotiation via message passing
- Integration of fixed_hours_template for cleaning/inventory shifts
- Preference scoring (employee-requested days off)
- Visualization dashboard (Streamlit/Gradio)
- Scalability to 1000+ stores

## 📄 License

MIT License – feel free to fork, improve, and use.

Built with ❤️ for the YEPAI AI Hackathon  
**"From Manual Chaos to Algorithmic Manager"**

---

1- Problem

Title: Employee Scheduling Problem

Description:

We need to schedule 40+ employees (crew + 6 managers) for 12 days.

Each day has different shifts: Breakfast, Lunch, Afternoon, Dinner, Closing.

Each shift has staff demand for different stations: kitchen, counter, mccafe, dessert.

Challenges / Constraints:

Employees may be unavailable on some days.

Max and min weekly hours.

Managers must be present on every shift.

Employees need enough rest between shifts.

Why it’s hard:

Many rules (hard and soft constraints) make manual scheduling slow and error-prone.

Shifts may be understaffed or overstaffed if done by hand.

Optional Visual:

A simple table showing missing coverage for some shifts.

2- Solution

Title: Multi-Agent Scheduler

We built a system with agents that work together to make the schedule automatically.

Agents and What They Do:

DemandForecastAgent → Calculates how many staff are needed for each shift.

AvailabilityAgent → Checks which employees are available.

SkillMatchingAgent → Matches employees to stations based on skills.

SchedulingOptimizerAgent → Assigns employees to shifts using optimization.

ConflictResolutionAgent → Fills gaps if some shifts are missing staff.

ComplianceVerifierAgent → Checks that hours and rules are respected.

Benefits:

All shifts covered, no conflicts.

Balanced work hours for employees.

Handles changes and unavailability automatically.

Generates a ready-to-use Excel schedule.

Optional Visuals:

Example of Excel schedule with shifts and total hours.

Bar chart of weekly hours for employees.

3- MAS Architecture

Title: How Our Agents Work Together

Diagram (Friendly Style):

[DemandForecastAgent] ----+
                          |
[AvailabilityAgent] ------> [SkillMatchingAgent] ---> [SchedulingOptimizerAgent] ---> [ConflictResolutionAgent] ---> [ComplianceVerifierAgent] ---> [Final Schedule]


Explanation:

Each agent passes information to the next agent.

Data is stored in a shared memory (weekly hours, shift assignments, conflicts).

If any shift is missing staff, the ConflictResolutionAgent fixes it.

The ComplianceVerifierAgent ensures all rules are followed before final output.
