import os

# Define all 8 S-Boxes (Using only one fully populated S-Box for now, update the rest)
S_BOXES = [
    # S1
    [
        [14, 4, 13, 1, 2, 15, 11, 8, 3, 10, 6, 12, 5, 9, 0, 7],
        [0, 15, 7, 4, 14, 2, 13, 1, 10, 6, 12, 11, 9, 5, 3, 8],
        [4, 1, 14, 8, 13, 6, 2, 11, 15, 12, 9, 7, 3, 10, 5, 0],
        [15, 12, 8, 2, 4, 9, 1, 7, 5, 11, 3, 14, 10, 0, 6, 13]
    ],
    # S2
    [
        [15, 1, 8, 14, 6, 11, 3, 4, 9, 7, 2, 13, 12, 0, 5, 10],
        [3, 13, 4, 7, 15, 2, 8, 14, 12, 0, 1, 10, 6, 9, 11, 5],
        [0, 14, 7, 11, 10, 4, 13, 1, 5, 8, 12, 6, 9, 3, 2, 15],
        [13, 8, 10, 1, 3, 15, 4, 2, 11, 6, 7, 12, 0, 5, 14, 9]
    ],
    # S3
    [
        [10, 0, 9, 14, 6, 3, 15, 5, 1, 13, 12, 7, 11, 4, 2, 8],
        [13, 7, 0, 9, 3, 4, 6, 10, 2, 8, 5, 14, 12, 11, 15, 1],
        [13, 6, 4, 9, 8, 15, 3, 0, 11, 1, 2, 12, 5, 10, 14, 7],
        [1, 10, 13, 0, 6, 9, 8, 7, 4, 15, 14, 3, 11, 5, 2, 12]
    ],
    # S4
    [
        [7, 13, 14, 3, 0, 6, 9, 10, 1, 2, 8, 5, 11, 12, 4, 15],
        [13, 8, 11, 5, 6, 15, 0, 3, 4, 7, 2, 12, 1, 10, 14, 9],
        [10, 6, 9, 0, 12, 11, 7, 13, 15, 1, 3, 14, 5, 2, 8, 4],
        [3, 15, 0, 6, 10, 1, 13, 8, 9, 4, 5, 11, 12, 7, 2, 14]
    ],
    # S5
    [
        [2, 12, 4, 1, 7, 10, 11, 6, 8, 5, 3, 15, 13, 0, 14, 9],
        [14, 11, 2, 12, 4, 7, 13, 1, 5, 0, 15, 10, 3, 9, 8, 6],
        [4, 2, 1, 11, 10, 13, 7, 8, 15, 9, 12, 5, 6, 3, 0, 14],
        [11, 8, 12, 7, 1, 14, 2, 13, 6, 15, 0, 9, 10, 4, 5, 3]
    ],
    # S6
    [
        [12, 1, 10, 15, 9, 2, 6, 8, 0, 13, 3, 4, 14, 7, 5, 11],
        [10, 15, 4, 2, 7, 12, 9, 5, 6, 1, 13, 14, 0, 11, 3, 8],
        [9, 14, 15, 5, 2, 8, 12, 3, 7, 0, 4, 10, 1, 13, 11, 6],
        [4, 3, 2, 12, 9, 5, 15, 10, 11, 14, 1, 7, 6, 0, 8, 13]
    ],
    # S7
    [
        [4, 11, 2, 14, 15, 0, 8, 13, 3, 12, 9, 7, 5, 10, 6, 1],
        [13, 0, 11, 7, 4, 9, 1, 10, 14, 3, 5, 12, 2, 15, 8, 6],
        [1, 4, 11, 13, 12, 3, 7, 14, 10, 15, 6, 8, 0, 5, 9, 2],
        [6, 11, 13, 8, 1, 4, 10, 7, 9, 5, 0, 15, 14, 2, 3, 12]
    ],
    # S8
    [
        [13, 2, 8, 4, 6, 15, 11, 1, 10, 9, 3, 14, 5, 0, 12, 7],
        [1, 15, 13, 8, 10, 3, 7, 4, 12, 5, 6, 11, 0, 14, 9, 2],
        [7, 11, 4, 1, 9, 12, 14, 2, 0, 6, 10, 13, 15, 3, 5, 8],
        [2, 1, 14, 7, 4, 10, 8, 13, 15, 12, 9, 0, 3, 5, 6, 11]
    ]
]


def read_file(file_path):
    "Reads the contents of a file and returns as a binary string."
    try:
        with open(file_path, 'rb') as file:
            content = file.read()
        binary_string = ''.join(format(byte, '08b') for byte in content)  # Convert bytes to binary
        return binary_string
    except FileNotFoundError:
        print(f"Error: File '{file_path}' not found.")
        return None

def s_box_substitution(expanded_text):
    "Applies S-Box substitution to the expanded 48-bit text."
    substituted_text = ""

    # Ensure we have 8 S-Boxes before processing
    if len(S_BOXES) < 8:
        print("Error: Not all 8 S-Boxes are defined.")
        return None

    # Divide input into 6-bit chunks and process each through its respective S-Box
    for i in range(8):  # We have 8 S-Boxes
        start = i * 6
        end = start + 6
        block = expanded_text[start:end]

        if len(block) != 6:
            print(f"Skipping incomplete block: {block}")
            continue

        row = int(block[0] + block[5], 2)  # First and last bit form the row
        col = int(block[1:5], 2)  # Middle four bits form the column

        s_box = S_BOXES[i]  # Use the correct S-Box

        if row >= len(s_box) or col >= len(s_box[0]):  # Check bounds
            print(f"Error: S-Box {i + 1} index out of range at Row {row}, Col {col}")
            return None

        s_box_value = s_box[row][col]  # Get value from the correct S-Box
        substituted_text += format(s_box_value, '04b')  # Convert to 4-bit binary

        print(f"S-Box {i + 1}: Bits = {block}, Row = {row}, Col = {col}, Output = {s_box_value}")

    return substituted_text

def des_encrypt(file_path):
    "Encrypts the file content using S-Box substitution (simplified DES process)."
    binary_text = read_file(file_path)

    if binary_text is None:
        return None

    print(f"Original binary text: {binary_text[:64]}...")  # Print first 64 bits

    expanded_text = binary_text[:48]  # Simplified selection (replace with actual expansion function)

    print(f"Expanded text: {expanded_text}")

    substituted_text = s_box_substitution(expanded_text)

    if substituted_text is None:
        print("S-Box substitution failed.")
        return None

    print(f"Substituted text: {substituted_text}")
    return substituted_text

# Run encryption
file_path = r"file path to input.txt"  # Ensure this file exists
encrypted_output = des_encrypt(file_path)

if encrypted_output:
    print(f"Final encrypted output: {encrypted_output}")
