# tradeshor.app

Vehicle trade-in bidding platform.

Owners list year/make/model/miles/condition. Dealers (any distance) submit firm offers pending physical inspection. Sell to highest bidder, buy your next vehicle anywhere.

## Files
- `tradeshor-landing.html` — Marketing landing page
- `tradeshor-prototype.html` — Interactive demo (listing form + bidding column + geolocation mock)
- `tradeshor-data-model.md` — Schema for owners, dealers, vehicles, bids, notifications
- `imagine_images/` — Logo, hero, social card, data model diagram (generate separately or add)

## MVP Stack Suggestion
1. Auth & Profiles → Supabase Auth
2. Vehicle Listing → Form → insert into vehicles table, status = 'up_for_bids'
3. Bidding Column → Realtime query on vehicles + bids
4. Geolocation → Browser API, store lat/lng, rank by distance but no hard limit
5. Notifications → Supabase + email/SMS (Resend/Twilio)
6. Hosting → Vercel or Hostinger; optional NHTSA VIN decode

Post your vehicle to get bids today.
