# Temporary profiles

`POST /profiles/temporary` creates a profile flagged `temporary`: it is hidden from `GET /profiles` and from the launcher UI, and is deleted (config + user-data-dir) automatically when its browser closes. If the app crashes while one is open, it is purged on the next app start.
