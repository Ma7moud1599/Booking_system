# Appointment Booking System

A full-stack appointment scheduling platform built with Laravel, Inertia.js, and Vue. Supports multi-employee, multi-service scheduling with real-time slot availability checking.

## Features

- **Employee & Service Management** — define employees, services they offer, and working schedules
- **Slot-based Availability** — precise time-slot generation with support for schedule exclusions (holidays, days off)
- **Appointment Booking** — customers select a service, pick an available employee, choose a slot, and confirm via checkout
- **UUID-based Appointment Access** — secure appointment lookup and cancellation without authentication
- **Stripe Checkout Integration** — payment step before appointment is confirmed
- **Inertia SPA** — seamless single-page experience powered by Inertia.js + Vue 3

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Laravel 11 |
| Frontend | Inertia.js + Vue 3 |
| Authentication | Laravel Breeze + Sanctum |
| Scheduling Logic | `spatie/period` |
| Routing (client) | Ziggy |
| Database | MySQL |

## Architecture

```
app/
├── Bookings/
│   ├── DateCollection.php          # Aggregates available dates
│   ├── ScheduleAvailability.php    # Checks schedule vs exclusions
│   ├── ServiceSlotAvailability.php # Merges employee + service slots
│   ├── SlotRangeGenerator.php      # Generates time slots for a date range
│   └── Slot.php                    # Value object: start/end time pair
├── Http/Controllers/
│   ├── HomeController.php
│   ├── EmployeeServiceIndexController.php
│   ├── CheckoutController.php
│   ├── AppointmentStoreController.php
│   ├── AppointmentShowController.php
│   └── AppointmentDestroyController.php
└── Models/
    ├── Employee.php
    ├── Service.php
    ├── Schedule.php
    ├── ScheduleExclusion.php
    └── Appointment.php
```

## Database Schema

| Table | Purpose |
|---|---|
| `employees` | Staff members |
| `services` | Bookable services with duration and price |
| `employee_service` | Pivot: which employees offer which services |
| `schedules` | Weekly working hours per employee |
| `schedule_exclusions` | Blocked date ranges (holidays, etc.) |
| `appointments` | Confirmed bookings with UUID identifier |

## Installation

```bash
git clone https://github.com/Ma7moud1599/Booking_system.git
cd Booking_system

composer install
npm install

cp .env.example .env
php artisan key:generate

# Configure your database in .env, then:
php artisan migrate --seed

npm run dev
php artisan serve
```

## Routes

| Method | URI | Description |
|---|---|---|
| GET | `/` | Home — list services |
| GET | `/employees/{employee:slug}` | Employee's available services |
| GET | `/checkout/{service}/{employee?}` | Checkout / slot picker |
| POST | `/appointments` | Create appointment |
| GET | `/appointments/{uuid}` | View appointment |
| DELETE | `/appointments/{uuid}` | Cancel appointment |

## License

MIT
