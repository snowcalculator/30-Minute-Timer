```python
def add_time(start, duration, start_day=None):

    # Split the start time into hour, minute, and period
    start_time_reformat = start.replace(":", " ")
    start_time_split = start_time_reformat.split()

    start_hour = int(start_time_split[0])
    start_minute = int(start_time_split[1])
    period = start_time_split[2].upper()

    # Validate start time
    if start_hour < 1 or start_hour > 12:
        return "Error: invalid start hour"

    if start_minute < 0 or start_minute >= 60:
        return "Error: invalid start minute"

    if period not in ("AM", "PM"):
        return "Error: invalid period"

    # Split duration into hours and minutes
    duration_split = duration.split(":")

    duration_hours = int(duration_split[0])
    duration_minutes = int(duration_split[1])

    # Duration minutes must be less than 60
    if duration_minutes >= 60:
        return "Error: duration minutes should be less than 60"

    # Convert the start time to 24-hour format
    if period == "AM":
        if start_hour == 12:
            start_military_hour = 0
        else:
            start_military_hour = start_hour
    else:
        if start_hour == 12:
            start_military_hour = 12
        else:
            start_military_hour = start_hour + 12

    # Convert everything into total minutes
    start_total_minutes = (
        start_military_hour * 60
        + start_minute
    )

    duration_total_minutes = (
        duration_hours * 60
        + duration_minutes
    )

    # Add the start time and duration
    final_total_minutes = (
        start_total_minutes
        + duration_total_minutes
    )

    # Calculate number of days passed
    days_passed = final_total_minutes // (24 * 60)

    # Calculate final time within the 24-hour day
    final_minutes_of_day = final_total_minutes % (24 * 60)

    final_hour_24 = final_minutes_of_day // 60
    final_minute = final_minutes_of_day % 60

    # Convert back to 12-hour format
    if final_hour_24 == 0:
        hour_final = 12
        final_period = "AM"

    elif final_hour_24 < 12:
        hour_final = final_hour_24
        final_period = "AM"

    elif final_hour_24 == 12:
        hour_final = 12
        final_period = "PM"

    else:
        hour_final = final_hour_24 - 12
        final_period = "PM"

    # Format minutes with a leading zero
    minutes_final = f"{final_minute:02d}"

    # Determine the day suffix
    if days_passed == 0:
        day_suffix = ""

    elif days_passed == 1:
        day_suffix = " (next day)"

    else:
        day_suffix = f" ({days_passed} days later)"

    # If a starting day was provided
    if start_day:

        days = [
            "sunday",
            "monday",
            "tuesday",
            "wednesday",
            "thursday",
            "friday",
            "saturday"
        ]

        start_day_lower = start_day.lower()

        if start_day_lower not in days:
            return "Error: invalid starting day"

        # Find the starting day's index
        start_day_index = days.index(start_day_lower)

        # Calculate the final day's index
        final_day_index = (
            start_day_index + days_passed
        ) % 7

        final_day_name = days[final_day_index].capitalize()

        # Build the final result
        new_time = (
            f"{hour_final}:{minutes_final} "
            f"{final_period}, {final_day_name}"
            f"{day_suffix}"
        )

    # No starting day was provided
    else:
        new_time = (
            f"{hour_final}:{minutes_final} "
            f"{final_period}"
            f"{day_suffix}"
        )

    return new_time
```
