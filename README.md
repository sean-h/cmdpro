# cmdpro

A command line parameter parser written in Rust.

## Example

```
extern crate cmdpro;

use cmdpro::{CommandLineProcessor, ParameterType, ParameterValue};

fn main() {
	// Create a new CommandLineProcessor
    let mut command_line_processor = CommandLineProcessor::new();
    
    // Add Parameters
    command_line_processor.add_parameter("path", ParameterType::Path, vec!["--path".to_owned(), "--p".to_owned()]);
    command_line_processor.add_parameter("value", ParameterType::UInteger, vec!["--value".to_owned(), "--v".to_owned()]);
    
    // Set the Help Text returned when the --help option is used.
    // include_str! can be used to use the text from a file.
    command_line_processor.set_help_text(include_str!("help_file.txt"));
    
    // Parse the command line parameters
    command_line_processor.parse_command_line();
    
	// If --help or --version option was used then exit.
    if command_line_processor.abort_flag() {
        return;
    }

	// Get the --path parameter
    let path = match command_line_processor.get_parameter_value("path") {
        ParameterValue::Path(path) => path,
        _ => panic!("Path not set"),
    };

	// Get the --value parameter
    let value = match command_line_processor.get_parameter_value("value") {
        ParameterValue::UInteger(value) => *value,
        _ => panic!("Value not set"),
    };

	// Print the parameter values
    println!("Path: {:?}", path);
    println!("Value: {}", value);
}

```

Calling the program with `cargo run -- --path ./Cargo.toml --value 5` returns the following:

```
Path: "./Cargo.toml"
Value: 5
```

Using the `--help` option with `cargo run -- --help` returns the text in `help_file.txt` (example below).
```
USAGE
    executable_name [OPTIONS]

OPTIONS
    --path      Path to a file
    --value     A u32 value
```
