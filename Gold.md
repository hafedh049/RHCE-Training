The `register` keyword in Ansible is used to capture the output of a task and store it in a variable. This allows you to use the output data in subsequent tasks, such as checking if a command was successful, processing the output, or making decisions based on the results.

### How `register` Works:

1. **Capture Output**: When you use `register` in a task, Ansible stores all the details about that task's execution in the variable you specify.
2. **Stored Data**: The variable contains a dictionary with multiple keys, providing detailed information about the task's execution, including:
   - `stdout`: The standard output of the command (if applicable).
   - `stderr`: The standard error output of the command (if applicable).
   - `rc`: The return code of the command (0 usually indicates success).
   - `changed`: A boolean indicating if the task resulted in a change.
   - `failed`: A boolean indicating if the task failed.
   - `msg`: The message or reason for failure if applicable.
   - `stdout_lines`: The standard output split into a list of lines.

3. **Using the Registered Variable**: You can then use the registered variable in subsequent tasks to make decisions or process the data.

### Example:

```yaml
- name: Run a command and register its output
  ansible.builtin.command: ls /some/directory
  register: ls_output

- name: Display the output
  ansible.builtin.debug:
    msg: "The directory contains: {{ ls_output.stdout_lines }}"

- name: Fail if the command did not run successfully
  ansible.builtin.fail:
    msg: "The directory listing failed."
  when: ls_output.rc != 0
```

### Explanation:

1. **`register: ls_output`**:
   - Captures the output of the `ls /some/directory` command and stores it in the `ls_output` variable.

2. **Accessing Registered Data**:
   - `ls_output.stdout_lines`: Lists the contents of the directory, split into individual lines.
   - `ls_output.rc`: Checks the return code of the command to see if it was successful.

3. **Conditional Logic**:
   - The `when` statement checks if the return code (`rc`) is not `0`. If it isn't, the playbook triggers a failure with a custom message.

### Summary:
- **`register`** is a powerful feature that enables you to capture and use task results dynamically in your playbooks.
- It allows you to make your playbooks more flexible, with conditional logic based on the results of previous tasks.

