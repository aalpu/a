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
@ExtendWith(MockitoExtension.class)
class MarkUpdateWireAndeceivableTaskletTest {

    @Tested MarkUpdateWireAndeceivableTasklet markUpdateWireAndeceivableTasklet;
    
    @Injectable StepContribution stepContribution;
    
    @Injectable ChunkContext chunkContext;
    
    @Injectable DataSource gosDataSource;
    
    @Injectable Environment environment;
    
    @Mocked SimpleJdbcCall simpleJdbcCall;

    @Test
    void execute() throws Exception {

        new Expectations() {{
            markUpdateWireAndeceivableTasklet.getSimpleJdbcCall(); result = simpleJdbcCall;

            simpleJdbcCall.withFunctionName("FUNC_COPY_UPDATED_CASHMATCHING_TO_AUDIT"); result = simpleJdbcCall;

            simpleJdbcCall.executeFunction(String.class); result = "expectedResult";
        }};

        // Execute the method under test
        markUpdateWireAndeceivableTasklet.execute(stepContribution, chunkContext);

        // Verify interactions or state if necessary
        new Verifications() {{
            simpleJdbcCall.executeFunction(String.class); times = 1;
        }};
    }
}
```
