import re

# Constants
MIN_LENGTH = 8
COMMON_PASSWORDS = ['123456', 'password', '123456789', 'qwerty', 'abc123', 'password1']

def check_length(password):
    """Check if password meets the minimum length requirement."""
    if len(password) < MIN_LENGTH:
        return False, f"Password must be at least {MIN_LENGTH} characters long."
    return True, "Password length is sufficient."

def check_complexity(password):
    """Check if password has at least one lowercase, one uppercase, one digit, and one special character."""
    if not re.search(r'[a-z]', password):
        return False, "Password must contain at least one lowercase letter."
    if not re.search(r'[A-Z]', password):
        return False, "Password must contain at least one uppercase letter."
    if not re.search(r'[0-9]', password):
        return False, "Password must contain at least one number."
    if not re.search(r'[@$!%*?&]', password):
        return False, "Password must contain at least one special character."
    return True, "Password complexity is good."

def check_uniqueness(password):
    """Check if password is not a common password."""
    if password in COMMON_PASSWORDS:
        return False, "Password is too common, please choose a different one."
    if len(set(password)) == 1:
        return False, "Password contains repeating characters, which is not secure."
    return True, "Password is unique."

def evaluate_password(password):
    """Evaluate the password's strength."""
    length_status, length_message = check_length(password)
    complexity_status, complexity_message = check_complexity(password)
    uniqueness_status, uniqueness_message = check_uniqueness(password)
    
    # Collect results
    results = {
        "Length Check": {"status": length_status, "message": length_message},
        "Complexity Check": {"status": complexity_status, "message": complexity_message},
        "Uniqueness Check": {"status": uniqueness_status, "message": uniqueness_message},
    }
    
    # Evaluate overall password strength
    overall_status = all([length_status, complexity_status, uniqueness_status])
    
    return overall_status, results

# Example usage:
if __name__ == "__main__":
    password = input("Enter your password for evaluation: ")
    status, feedback = evaluate_password(password)
    
    print("\nPassword Evaluation Results:")
    for check, result in feedback.items():
        print(f"{check}: {'PASS' if result['status'] else 'FAIL'} - {result['message']}")
    
    if status:
        print("\nYour password is strong!")
    else:
        print("\nYour password is weak, please improve it.")
