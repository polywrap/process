# Datetime Wrapper Interface

| Version | URI | WRAP Version |
|-|-|-|
| 1.0.0 | [`wrap://ens/wraps.eth:datetime@1.0.0`](https://wrappers.io/v/ens/wraps.eth:datetime@1.0.0) | 0.1 |

## Interface
```graphql
type Module {
    # Starts a new process with given command and arguments. Returns the process ID.
    start(command: String!, args: [String], config: ProcessConfig): Int!

    # Waits for the process to exit and returns the exit code.
    waitForExit(pid: Int!): Int!

    # Sends a signal to the process.
    sendSignal(pid: Int!, signal: ProcessSignal!): Boolean!

    # Writes data to the process's standard input.
    writeStdin(pid: Int!, data: String!): Boolean!

    # Reads the process's standard output.
    readStdout(pid: Int!): String

    # Reads the process's standard error.
    readStderr(pid: Int!): String

    # Terminates the process forcefully.
    kill(pid: Int!): Boolean!

    # Attempts to terminate the process gracefully.
    terminate(pid: Int!): Boolean!

    # Suspends the process.
    suspend(pid: Int!): Boolean!

    # Resumes the process after being suspended.
    resume(pid: Int!): Boolean!

    # Returns the current state of the process
    state(pid: Int!): ProcessState

    # Returns the exit code of the process if it has already exited.
    exitCode(pid: Int!): Int

    # Returns the exit reason of the process if it has already exited.
    exitReason(pid: Int!): String

    # Returns comprehensive information about a process
    info(pid: Int!): ProcessInfo

    # Clear data stored about processes that have already exited
    gc: Boolean!
}

type ProcessConfig {
    # Current working directory
    cwd: String
    # Environmental Variables
    env: JSON
    # StdIn configuration
    stdin: StdIO
    # StdOut configuration
    stdout: StdIO
    # StdErr configuration
    stderr: StdIO
}

type StdIO {
    # Configure IO stream
    strategy: IOStrategy!
    # Absolute path of file to redirect to/from. Creates file if necessary. Required if action is REDIRECT, ignored otherwise.
    redirectFilePath: String
}

enum IOStrategy {
    # IO can be accessed with methods writeStdIn, readStdOut, readStdErr
    PIPE
    # IO is forwarded to parent process
    INHERIT
    # IO is ignored
    IGNORE
    # IO is redirected to/from a file
    REDIRECT
}

type ProcessInfo {
    # process id
    pid: Int!
    # process initialization command
    cmd: String!
    # process arguments
    args: [String]
    # process configuration
    config: ProcessConfig
    # current state of the process
    state: ProcessState!
    # Start time of the process as a unix timestamp
    startTime: Int!
    # Total running time of the process
    totalTimeMs: Int!
    # Total CPU time used by the process
    totalCpuTimeMs: Int!
}

enum ProcessState {
    RUNNING
    SUSPENDED
    EXITED
    UNKNOWN
}

enum ProcessSignal {
    # Hangup detected on controlling terminal or death of controlling process
    SIGHUP
    # Interrupt from keyboard
    SIGINT
    # Quit from keyboard
    SIGQUIT
    # Abort signal from abort(3)
    SIGABRT
    # Kill signal
    SIGKILL
    # Timer signal from alarm(2)
    SIGALRM
    # Termination signal
    SIGTERM
    # User-defined signal 1
    SIGUSR1
    # User-defined signal 2
    SIGUSR2
    # Continue if stopped
    SIGCONT
    # Stop process
    SIGSTOP
    # Stop typed at terminal
    SIGTSTP
    # Terminal input for background process
    SIGTTIN
    # Terminal output for background process
    SIGTTOU
}
```

## Usage
```graphql
#import * from "ens/wraps.eth:process@1.0.0"
```

And implement the interface methods within your programming language of choice.

## Source Code
[Link](https://github.com/polywrap/std/process)

## Known Implementations
[Link](https://github.com/polywrap/process/tree/master/implementations)