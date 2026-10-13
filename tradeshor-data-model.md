# tradeshor.app Data Model

## Core Entities

### owners
- id: UUID (PK)
- email: string (unique)
- phone: string
- name: string
- location_lat: float (geolocation)
- location_lng: float
- location_zip: string
- created_at: timestamp
- updated_at: timestamp

### dealers
- id: UUID (PK)
- email: string (unique)
- business_name: string
- phone: string
- address: string
- city: string
- state: string
- zip: string
- license_number: string (optional verification)
- location_lat: float
- location_lng: float
- is_verified: boolean
- is_premier: boolean (default false)
- premier_radius_miles: integer (15–30)
- preferred_makes_models: jsonb (array of up to 5 make/model pairs)
- created_at: timestamp

### vehicles
- id: UUID (PK)
- owner_id: UUID (FK → owners)
- year: integer
- make: string
- model: string
- trim: string
- miles: integer
- condition: enum ('excellent', 'good', 'fair', 'poor')
- title_status: enum ('clean', 'salvage', 'rebuilt', 'other')
- vin: string (optional)
- notes: text
- photos: jsonb (array of urls)
- status: enum ('draft', 'up_for_bids', 'offer_accepted', 'inspected', 'sold', 'expired')
- negative_equity_flag: boolean (optional self-reported or calculated)
- created_at: timestamp
- updated_at: timestamp

### bids
- id: UUID (PK)
- vehicle_id: UUID (FK → vehicles)
- dealer_id: UUID (FK → dealers)
- amount: decimal(10,2)
- notes: text (optional conditions)
- status: enum ('pending', 'accepted', 'rejected', 'expired')
- expires_at: timestamp (e.g. 48-72 hours)
- created_at: timestamp

### notifications
- id: UUID (PK)
- user_id: UUID (owner or dealer)
- type: enum ('new_bid', 'offer_accepted', 'inspection_scheduled', 'message')
- message: text
- is_read: boolean
- created_at: timestamp

## Relationships
- One owner → many vehicles
- One vehicle → many bids
- One dealer → many bids
- Geolocation used for relevance ranking but not hard filtering (dealers any distance)
- Premier dealers receive early alerts for matching vehicles within 15–30 miles of their location (up to 5 preferred make/model pairs). $75 fee per vehicle purchased.

## Tech Notes (MVP)
- Auth: Supabase Auth or Firebase Auth (email + magic link / OTP)
- Database: Postgres (Supabase) with Row Level Security
- Realtime: Supabase Realtime for live bid updates
- Geolocation: Browser Geolocation API + reverse geocode to zip/city
- Notifications: Supabase + Resend/Twilio for email/SMS
- VIN decode: NHTSA free API or paid service
- Hosting: Vercel / Hostinger / Replit as preferred
