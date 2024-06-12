```
import pyautogui
import time
import pyperclip

def read_server_details(file_path):
    with open(file_path, 'r') as file:
        servers = [line.strip() for line in file.readlines()]
    return servers

def open_remote_desktop(hostname, password):
    # Open Remote Desktop Connection
    pyautogui.press('win')
    time.sleep(1)
    pyautogui.write('Remote Desktop Connection')
    time.sleep(1)
    pyautogui.press('enter')
    time.sleep(3)

    # Fill in the computer details
    pyautogui.write(hostname)
    time.sleep(1)
    pyautogui.press('enter')
    time.sleep(10)  # Adjust the sleep time based on your network speed

    # Enter user password
    pyautogui.write(password)
    time.sleep(1)
    pyautogui.press('enter')
    time.sleep(10)  # Adjust the sleep time for login

def run_command_on_server():
    # Open CMD
    pyautogui.press('win')
    time.sleep(1)
    pyautogui.write('cmd')
    time.sleep(1)
    pyautogui.press('enter')
    time.sleep(2)
    
    # Navigate to directory
    cmd = 'cd /d D:\\Program Files (x86)\\CyberArk\\ApplicationPasswordSdk'
    pyautogui.write(cmd)
    pyautogui.press('enter')
    time.sleep(1)
    
    # Run the command and get output
    command = 'XYZ'
    pyautogui.write(command)
    pyautogui.press('enter')
    time.sleep(3)
    
    # Copy the command output
    pyautogui.hotkey('ctrl', 'a')
    time.sleep(1)
    pyautogui.hotkey('ctrl', 'c')
    time.sleep(1)
    output = pyperclip.paste()

    # Process output to extract the actual command result, skipping the first two lines
    output_lines = output.splitlines()
    command_output = '\n'.join(output_lines[2:])  # Skip the first two lines

    return command_output

def automate_servers(file_path, password):
    servers = read_server_details(file_path)
    outputs = []

    for i, server in enumerate(servers):
        print(f'Processing server {i+1}/{len(servers)}: {server}')
        open_remote_desktop(server, password)
        output = run_command_on_server()
        outputs.append(output)
        
        # Close the Remote Desktop session (assumes session is still in focus)
        pyautogui.hotkey('alt', 'f4')
        time.sleep(2)
    
    return outputs

def compare_outputs(outputs):
    return all(output == outputs[0] for output in outputs)

def generate_report(outputs, comparison_result):
    report = 'Server Outputs:\n'
    for i, output in enumerate(outputs):
        report += f'Server {i+1} output: {output}\n'
    report += '\nComparison Result:\n'
    report += 'All servers outputs are the same: ' + str(comparison_result) + '\n'
    return report

# Main function
if __name__ == "__main__":
    file_path = 'server_details.txt'
    password = 'your_password'
    
    outputs = automate_servers(file_path, password)
    comparison_result = compare_outputs(outputs)
    report = generate_report(outputs, comparison_result)
    
    with open('report.txt', 'w') as report_file:
        report_file.write(report)
    
    print('Report generated: report.txt')
```

