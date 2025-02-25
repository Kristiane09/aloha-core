import re
from datetime import datetime

def validate_nric(nric):
    # NRIC format: YYMMDD-XX-YYYY
    pattern = re.compile(r'^\d{6}-\d{2}-\d{4}$')
    if not pattern.match(nric):
        return False

    # Extract date of birth
    dob = nric[:6]
    try:
        birth_date = datetime.strptime(dob, '%y%m%d')
        if birth_date > datetime.now():
            birth_date = birth_date.replace(year=birth_date.year - 100)
    except ValueError:
        return False

    # Check validity of the state code (XX)
    state_code = int(nric[7:9])
    valid_state_codes = list(range(1, 17)) + list(range(21, 24)) + [30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
    if state_code not in valid_state_codes:
        return False

    return True

# Example usage
nric = "930509-28-0809"
if validate_nric(nric):
    print("Valid NRIC")
else:
    print("Invalid NRIC")