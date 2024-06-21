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
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;
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
    private DataSource dataSource;

    @BeforeEach
    void setUp() {
        // Use a spy to allow partial mocking of the gosDatabaseConfiguration instance
        gosDatabaseConfiguration = Mockito.spy(new GOSDatabaseConfiguration());

        // Mock environment properties that would be used by the configuration
        when(environment.getProperty("FUNCTIONAL_ACCOUNT")).thenReturn("functionalAccount");
        when(environment.getProperty("java.security.krb5.conf")).thenReturn("krb5Conf");
        when(environment.getProperty("oracle.net.kerberos5_cc_name")).thenReturn("kerberos5CcName");

        // Mock the DataSource creation method if it's defined within the class
        doReturn(dataSource).when(gosDatabaseConfiguration).createDataSource();
    }

    @Test
    void createGOSDatasource() {
        // Call the method under test
        DataSource result = gosDatabaseConfiguration.createGOSDatasource();

        // Verify interactions with environment
        verify(environment).getProperty("FUNCTIONAL_ACCOUNT");
        verify(environment).getProperty("java.security.krb5.conf");
        verify(environment).getProperty("oracle.net.kerberos5_cc_name");

        // Verify that the createDataSource method was called and the result is the mocked dataSource
        verify(gosDatabaseConfiguration).createDataSource();
        assertSame(dataSource, result);
    }

    @Test
    void getNamedTemplateForGOS() {
        // Mock the getNamedTemplateForGOS() method if needed
        when(gosDatabaseConfiguration.getNamedTemplateForGOS()).thenReturn(namedParameterJdbcTemplate);

        // Call the method under test
        NamedParameterJdbcTemplate result = gosDatabaseConfiguration.getNamedTemplateForGOS();

        // Verify that the method returns the mocked namedParameterJdbcTemplate
        assertSame(namedParameterJdbcTemplate, result);
    }
}

```
