# Parking Slot Management System

A full stack web app to manage vehicle check-in and check-out in a parking lot, with automatic fee calculation.

**Tech stack:** React (Vite), Python Flask, SQLite (SQL), HTML, CSS, JavaScript

## Features
- Check in a vehicle (automatically assigned the first free slot)
- Check out a vehicle with automatic fee calculation (rate per hour, rounded up to the next hour, minimum 1 hour)
- Live slot grid showing free and occupied slots
- Dashboard: free slots, occupied slots, earnings today
- Currently parked vehicles and recent visit history
- Validation: no duplicate vehicles, no check-in when parking is full

## Project structure
```
parking-management/
  backend/
    app.py             Flask REST API + SQLite
    requirements.txt
  frontend/
    index.html
    package.json
    vite.config.js
    src/
      main.jsx         React entry point
      App.jsx          Main component (state, fetch calls, UI)
      App.css          Styles
```

## Database
```
slots(id, slot_number, is_occupied)
parking(id, slot_id, vehicle_number, entry_time, exit_time, fee)
```
`parking.slot_id` is a foreign key to `slots.id`. A visit with `exit_time = NULL` means the vehicle is still parked.

## API endpoints
| Method | URL                      | Purpose                                  |
|--------|--------------------------|------------------------------------------|
| GET    | /api/slots               | All slots with parked vehicle (LEFT JOIN)|
| POST   | /api/checkin             | Park a vehicle in the first free slot    |
| PUT    | /api/checkout/<id>       | Check out, calculate fee, free the slot  |
| GET    | /api/parked              | Currently parked vehicles (JOIN)         |
| GET    | /api/history             | Last 10 completed visits                 |
| GET    | /api/summary             | Free, occupied, earnings today           |

## How to run

### 1. Backend (Terminal 1)
```bash
cd backend
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```
The API runs at http://127.0.0.1:5000

### 2. Frontend (Terminal 2)
```bash
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

## Fee rule
`fee = hours x rate`, where hours is the parked time rounded up to the next hour (minimum 1). The rate is set by `RATE_PER_HOUR` in `backend/app.py`.
