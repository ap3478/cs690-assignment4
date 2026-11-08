Use the a4-tools MCP server: call its run_tests tool, then its read_file tool on discounts.py, and tell me why the failing test fails. Do not change any files

  Called a4-tools 2 times

One of the 4 tests fails: test_lowercase_code_works, with ValueError: unknown code: save10.

Why: In discounts.py, the codes in CODES are stored in uppercase ("SAVE10", "WELCOME5"). Line 16 checks the code exactly as it was passed in:

if code not in CODES:
    raise ValueError(f"unknown code: {code}")

The test passes "save10", and Python dictionary lookups are case-sensitive, so "save10" doesn't match "SAVE10". The function raises ValueError before it applies any discount. The test expects codes to work in any case, but the function never converts the input to uppercase (with code.upper() or something like it) before the lookup and before reading CODES[code] on line 18.

Something else I noticed: last_day is read on line 18 but never used, so the function doesn't check whether a code has expired. None of the current tests catch this, but it's probably a second bug.

I didn't change any files.