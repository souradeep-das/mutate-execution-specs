1. convert the result of fetch_tests to a list when passing to pytest parameterize, otherwise mutmut complains

2. Skip specific tests with excessively deep execution paths that cannot be reliably run under mutmut.

3. Setup files for mutmut forced fail check.

4. Logic to skip fixture locking and downloading for mutmut child processes 