
```
### Weekly Progress Report

#### Java and Photon Migration of Trade Settlement Cash Matching Batch
- Successfully upgraded Photon from 2.7 to 3.0
- Successfully upgraded Photon from 3.0 to 3.1
- Upgrade to Photon 3.2 is in progress (currently addressing errors related to list and chunk, and updating test cases)
- Fixed errors in JUnit test cases
- Created new test cases from scratch as the previous ones were non-functional despite updates to annotations and code

#### Java and Photon Migration of Trade Settlement Cash Matching
- Java upgrade from version 11 to 17 is in progress

#### L2: EPV ID Validation
- Provided a demo to Kananan and Hajira

```
```
import re

def extract_info(file_path):
    # Define the regular expression patterns for the required fields
    safe_pattern = r'safe\s*=\s*([^,]+)'
    folder_pattern = r'folder\s*=\s*([^,]+)'
    name_pattern = r'name\s*=\s*([^]]+)'
    appid_pattern = r'application\s*\[\s*([^]]+)\s*\]'
    
    with open(file_path, 'r') as file:
        # Read the first line of the file
        first_line = file.readline()
        
        # Extract the required fields using regular expressions
        safe_match = re.search(safe_pattern, first_line)
        folder_match = re.search(folder_pattern, first_line)
        name_match = re.search(name_pattern, first_line)
        appid_match = re.search(appid_pattern, first_line)
        
        # Extract the values from the match objects
        safe = safe_match.group(1) if safe_match else None
        folder = folder_match.group(1) if folder_match else None
        name = name_match.group(1) if name_match else None
        appid = appid_match.group(1) if appid_match else None
        
        return {
            'safe': safe,
            'folder': folder,
            'name': name,
            'appid': appid
        }

# Example usage
file_path = 'path_to_your_log_file.log'
info = extract_info(file_path)
print(info)

```



```
https://sequencediagram.org/index.html#initialData=C4S2BsFMAIEEFdgHsC2BDYMAKaDOuB3JAJwBNoAxSYAYwAto0A7cgYVQAc1iRckmAUALQ1kxaAFVckYgK7FQNEFybBoAZRo8OwOd0XLmaiiCjqAnrkwo9CkEpVr18AEYdiSGpHxCpMgLQAfJrawABc0ABK8EyMiKgYkAD6uCBMAOZQKTIAbjIAFACUAiHKwEEmZpbWEax0kDQA1tAgAGZwHBwIpGAAdOBI6dCQAB68wLjC4Gqwnd19A0Oj45PQaxpaZRWmkBZWkCgRkZBo5K0gxFbQ4GkwSO2zXfA9wP2DAusboUGlOhE4lxgj3mr0WH3WaGm0AAMoNoK0SOg1DlISBSODPmtfuVgps-tBWMQTphoFx8EQyNAaKh0CwMZjsT9XO5PN5cBEAPIcSCxVgAWQAIowWNBiDEqTTmOjMetnG4PF58D88eFoBzEBxEKLvPBpvTPozcaEIgBREbAYgiNRkwgkM4eFDQJAaxD62Uq7ZVfaHAn1JrC8hWEgwG0U6Uyyg7PbWT27aoHI4ncgmrAANSSOHJdt6wHNbrWkLU0LZ0GAdGY0AAHMNVDxvPnMZU497Y9GEx1uSKmJACKS8LbKTyLeYG5BwNIqzWLSB6xGGR6jWUIgBxHkyRLajgkXRzg0Lpttn2sKDcaAp9OZgekHN53fu76BA-xn0AdR4JKJW4UpaQm+3N53CMeXDNYxwnWEhgRYgkRaJgURuED5wfPxiH+HhVGgAAiCg0B2chkD7QEOxBN50kw+lgIEMCgTmZ4FjhUgkG8aAmCQNRliselDRQtC0jUTDgTo0E4V4Fi2MYFFTDQFwoEYNQyxgAYaAwEB+HIyigA
```
