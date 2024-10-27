# Spotify Penetration Analysis

## Roles
- Data Engineer
- Data Scientist
- BI Analyst
- Data Analyst
- ML Engineer
- Software Engineer

## Introduction
Market penetration is an important metric for understanding Spotify's performance and growth potential in different regions. You are part of the analytics team at Spotify and are tasked with calculating the active user penetration rate in specific countries.

## Active Users Criteria
For this task, 'active_users' are defined based on the following criteria:
- **last_active_date**: The user must have interacted with Spotify within the last 30 days.
- **sessions**: The user must have engaged with Spotify for at least 5 sessions.
- **listening_hours**: The user must have spent at least 10 hours listening on Spotify.

## Calculation
Based on the conditions above, calculate the active 'user_penetration_rate' by using the following formula:

**Active User Penetration Rate** = (Number of Active Spotify Users in the Country / Total users in the Country)

Total Population of the country is based on both active and non-active users.

## Output
The output should contain:
- **country**
- **active_user_penetration_rate** (rounded to 2 decimals)

## Assumption
Let's assume the current_day is 2024-01-31.

## Example Query
```sql
WITH ActiveUsers AS (
    SELECT 
        country,
        COUNT(user_id) AS active_users
    FROM 
        spotify_users
    WHERE 
        last_active_date >= '2024-01-01' 
        AND sessions >= 5
        AND listening_hours >= 10
    GROUP BY 
        country
),
TotalUsers AS (
    SELECT 
        country,
        COUNT(user_id) AS total_users
    FROM 
        spotify_users
    GROUP BY 
        country
)
SELECT 
    a.country,
    ROUND((CAST(a.active_users AS FLOAT) / t.total_users) * 100, 2) AS active_user_penetration_rate
FROM 
    ActiveUsers a
JOIN 
    TotalUsers t
ON 
    a.country = t.country;

