```
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.*;

import java.time.LocalDateTime;
import java  util.Collections;
import java.util.List;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.core.JobInstance;
import org.springframework.batch.core.StepExecution;
import org.springframework.core.env.Environment;

class CashMatchingAuditBatchListenerTest {

    @InjectMocks
    private CashMatchingAuditBatchListener listener;

    @Mock
    private GosDAO gosDAO;

    @Mock
    private Environment environment;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testBeforeJob_whenNoPreviousBatch() {
        // Mock behavior
        when(gosDAO.getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT)).thenReturn(null);
        when(environment.getProperty("FUNCTIONAL_ACCOUNT")).thenReturn("testUser");

        // Execute the method
        listener.beforeJob(new JobExecution(new JobInstance(1L, "testJob"), null));

        // Verify interactions
        verify(gosDAO).getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT);
        verify(environment).getProperty("FUNCTIONAL_ACCOUNT");
        
        // Assert data is set correctly on a new BatchControl object
        verify(gosDAO).insertUpdateBatchStatus(any(BatchControl.class));
    }

    @Test
    void testBeforeJob_withPreviousBatch() {
        // Mock behavior
        BatchControl previousBatch = new BatchControl();
        previousBatch.setJobName("previousJob");
        previousBatch.setStatus(BatchStatusEnum.COMPLETED);
        when(gosDAO.getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT)).thenReturn(previousBatch);

        // Execute the method
        listener.beforeJob(new JobExecution(new JobInstance(1L, "testJob"), null));

        // Verify interactions
        verify(gosDAO).getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT);
        verify(environment, never()).getProperty("FUNCTIONAL_ACCOUNT"); // Not called for existing batch
        
        // Assert no update is done to the previous BatchControl object
        verify(gosDAO, never()).insertUpdateBatchStatus(previousBatch);
    }

    @Test
    void testAfterJob_noExceptions() {
        // Mock behavior
        JobExecution jobExecution = mock(JobExecution.class);
        JobInstance jobInstance = mock(JobInstance.class);
        when(jobExecution.getJobInstance()).thenReturn(jobInstance);
        when(jobInstance.getJobName()).thenReturn("testJob");
        when(jobExecution.getStepExecutions()).thenReturn(Collections.emptyList());

        // Execute the method
        listener.afterJob(jobExecution);

        // Verify interactions
        verify(gosDAO).getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT);
        verify(jobExecution).getJobInstance();
        verify(jobInstance).getJobName();
        verify(jobExecution).getStepExecutions();
        verify(gosDAO).insertUpdateBatchStatus(any(BatchControl.class));

        // Assert no error message or record count is set
        verify(gosDAO, never()).setErrorMessage(any());
        verify(gosDAO, never()).setRecordCount(any());
    }

    @Test
    void testAfterJob_withExceptions() {
        // Mock behavior
        JobExecution jobExecution = mock(JobExecution.class);
        JobInstance jobInstance = mock(JobInstance.class);
        StepExecution stepExecution = mock(StepExecution.class);
        List<Throwable> exceptions = Collections.singletonList(new RuntimeException("test exception"));
        when(jobExecution.getJobInstance()).thenReturn(jobInstance);
        when(jobInstance.getJobName()).thenReturn("testJob");
        when(jobExecution.getStepExecutions()).thenReturn(Collections.singletonList(stepExecution));
        when(stepExecution.getFailureExceptions()).thenReturn(exceptions);

                listener.afterJob(jobExecution);

        // Verify interactions
        verify(gosDAO).getLatestBatchRun(BatchTypeEnum.CASHMATCHING_AUDIT);
        verify(jobExecution).getJobInstance();
        verify(jobInstance).getJobName();
        verify(jobExecution).getStepExecutions();
        verify(gosDAO).insertUpdateBatchStatus(any(BatchControl.class));

        // Assert error message is set
        verify(gosDAO).setErrorMessage(any());
        String allExceptionsMessage = exceptions.stream().map(Throwable::getMessage).collect(joining(";"));
        verify(gosDAO).setErrorMessage(allExceptionsMessage);

        // Assert record count is set with step execution times
        verify(gosDAO).setRecordCount(any());
        StringBuilder expectedRecordCount = new StringBuilder();
        expectedRecordCount.append("Job Name: ").append(jobExecution.getJobInstance().getInstanceId()).append("\n");
        expectedRecordCount.append(stepExecution.getStepName()).append(" took 0 seconds \n"); // Mock doesn't provide real time
        expectedRecordCount.append("-----------------");
        verify(gosDAO).setRecordCount(expectedRecordCount.toString());
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
