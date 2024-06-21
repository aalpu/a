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
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.core.env.Environment;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;

import javax.sql.DataSource;

import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class GOSDatabaseConfigurationTest {

    @InjectMocks
    private GOSDatabaseConfiguration gosDatabaseConfiguration;

    @Mock
    private Environment environment;

    @Mock
    private NamedParameterJdbcTemplate namedParameterJdbcTemplate;

    @Mock
    private SimpleJdbcCall simpleJdbcCall;

    @Mock
    private DataSource dataSource;

    // Injecting properties
    @Mock
    private long connectionTimeoutMs;

    @Mock
    private long idleTimeoutMs;

    @Mock
    private int maximumPoolSize;

    @Mock
    private int minimumIdle;

    @Mock
    private long maxLifetimeMs;

    @BeforeEach
    void setUp() {
        // Use a spy to allow partial mocking of the gosDatabaseConfiguration instance
        gosDatabaseConfiguration = Mockito.spy(gosDatabaseConfiguration);

        // Mock the getSimpleJdbcCall method to return the mock simpleJdbcCall
        Mockito.doReturn(simpleJdbcCall).when(gosDatabaseConfiguration).getSimpleJdbcCall();
    }

    @Test
    void createGOSDatasource() {
        // Mock environment properties
        Mockito.when(environment.getProperty("FUNCTIONAL_ACCOUNT")).thenReturn("anyString");
        Mockito.when(environment.getProperty("java.security.krb5.conf")).thenReturn("anyString");
        Mockito.when(environment.getProperty("oracle.net.kerberos5_cc_name")).thenReturn("anyString");

        // Mock HikariDataSource creation
        HikariDataSource mockDataSource = Mockito.mock(HikariDataSource.class);
        Mockito.when(new HikariDataSource(Mockito.any(HikariConfig.class))).thenReturn(mockDataSource);

        // Call the method under test
        gosDatabaseConfiguration.createGOSDatasource();

        // Verify interactions or assert results as needed
        verify(environment).getProperty("FUNCTIONAL_ACCOUNT");
        verify(environment).getProperty("java.security.krb5.conf");
        verify(environment).getProperty("oracle.net.kerberos5_cc_name");
    }

    @Test
    void getNamedTemplateForGOS() {
        // Implement test case for getNamedTemplateForGOS if needed
        // Example:
        // gosDatabaseConfiguration.getNamedTemplateForGOS();
    }
}
```
