```
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.core.JobInstance;
import org.springframework.batch.core.StepExecution;
import org.springframework.core.env.Environment;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Collections;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class CashMatchingAuditBatchListenerTest {

    @Mock
    private GosDAO gosDAO;

    @Mock
    private Environment environment;

    @Mock
    private JobExecution jobExecution;

    @Mock
    private JobInstance jobInstance;

    @Mock
    private StepExecution stepExecution;

    @InjectMocks
    private CashMatchingAuditBatchListener listener;

    private BatchControl currentBatchObject;

    @BeforeEach
    void setUp() {
        currentBatchObject = new BatchControl();
        currentBatchObject.setJobName("TestJob");
        when(gosDAO.getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT)).thenReturn(currentBatchObject);
        when(environment.getProperty("FUNCTIONAL_ACCOUNT")).thenReturn("testUser");
        when(jobExecution.getJobInstance()).thenReturn(jobInstance);
        when(jobInstance.getJobName()).thenReturn("TestJobName");
    }

    @Test
    void testBeforeJob() {
        listener.beforeJob(jobExecution);
        
        verify(gosDAO, times(1)).insertUpdateBatchStatus(currentBatchObject);
        verify(jobExecution, times(1)).getJobInstance();
        verify(environment, times(1)).getProperty("FUNCTIONAL_ACCOUNT");
    }

    @Test
    void testAfterJob() {
        when(jobExecution.getStepExecutions()).thenReturn(Collections.singletonList(stepExecution));
        when(stepExecution.getFailureExceptions()).thenReturn(Collections.emptyList());
        when(jobInstance.getInstanceId()).thenReturn(1L);
        when(stepExecution.getStartTime()).thenReturn(LocalDateTime.now());
        when(stepExecution.getEndTime()).thenReturn(LocalDateTime.now().plusSeconds(10));
        when(jobExecution.getLastUpdated()).thenReturn(java.util.Date.from(LocalDateTime.now().atZone(ZoneId.systemDefault()).toInstant()));

        listener.afterJob(jobExecution);
        
        verify(gosDAO, times(1)).insertUpdateBatchStatus(currentBatchObject);
        verify(jobExecution, times(1)).getJobInstance();
        verify(stepExecution, times(1)).getFailureExceptions();
    }
}

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
(https://sequencediagram.org/index.html#initialData=C4S2BsFMAIEEFdgHsC2BDYMAKaDOuB3JAJwBNoAxSYAYwAto0A7cgYVQAc1iRckmAUALQ1kxaAFVckYgK7FQNEFybBoAZRo8OwOd0XLmagPLq9CkEpVrWAWQAi5g9cogo6gJ65MKAVJkAtAB8mtrAAFzQAErwTNC4IEwA5lAA+tLEAG4yABQAlAKhysDBFG6Qnt6QKJGsdJA0ANbQIABmcBwcCKRgAHTgSEnQkAAevMC4wuBqsJ3dfQNDo+OT0GsaWsWl5ZU+kVGQaOStIMTe0OCJMEjts13wPcD9gwLrG2HBRTqROGcwd-MnotXus0NNoAAZQbQVokdBqTJgkCkEFvNZfEohTbfaCsYiHTDQLj4IhkaA0VDoFiotEY4KmSLGDiQOJ2Rxo9amYJsxnM1kOGlvOlBHnQAByaEyICSGBgyA09gA0tAevjRCQPIL1sLRQBREYNRAwYmEEjkCkoKkojlrNmfbERaDGRAcRDQfG4eDTLXoh32sKRfXAYgiNQm0nHYioaBIF2IH3vLZBMruLx7XH1JqMFjxMTGvCmsgJlMVNPVbapqo1aKHci6rAANV6wBGuhtjHBEMg+GgwDozGgAA5hqoeN2E28S7ty8mdmXq3cWeQmJACESCxGR8HNe3IOBpEOt2PVu3tX6sQHoABxFkyWXuyAcEht0++j6zyvp1hQbjQetNlsX1fYUp3nSIAHUeEJfEnwUXskAfWCnkAhMlxpPcDyhIZYWIeEWiYRFLmtG1hX8Ygfh4VRoAAIgoNBynIeV5APAEHgWQZqPQ6kMP+OY2KBaFSCQbtoCYJA1GWbwaVIjIKMSNRqNYx5niGXhRPExhETcNAACMoEYNQ+xgAYaAwEB+E4tCgA)



```
