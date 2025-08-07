# Flox Java Issue

_It's recommended to open this repository in a Devcontainers environment such as GitHub Codespaces or VS Code._

## Host

This Ubuntu environment defines a value for the `JAVA_HOME` environment variable. Let's verify that by running the following command:

```bash
echo $JAVA_HOME
```

The result will be `/yolo`.

## Nix Flakes

Let's see what happens when we use Nix Flakes to define a Java development environment.

Navigate to the `graalvm-ce` directory:

```bash
cd graalvm-ce
```

Then, activate the Nix development environment:

```bash
nix develop
```

Now, print out the JAVA_HOME environment variable:

```bash
echo $JAVA_HOME
```

It will correctly point to the Nix store path, such as `/nix/store/abc123-graalvm-ce-24.0.1`.

Exit the environment and go back to the root directory:

```bash
exit
cd ..
```

Try printing the `JAVA_HOME` variable again:

```bash
echo $JAVA_HOME
```

It will still print `/yolo`, which is the value defined in the current Ubuntu environment, not the Nix environment. This is expected behavior since the Nix Flakes environment does not modify the host's environment variables.

## Flox

Let's now see what happens when we use Flox instead of plain Nix Flakes.

First, install Flox following [these instructions](https://flox.dev/docs/install-flox/install/#__tabbed_1_3).

Next, navigate to the `flox` directory:

```bash
cd flox
```

Then, activate the Nix development environment:

```bash
flox activate
```

Print out the JAVA_HOME environment variable:

```bash
echo $JAVA_HOME
```

It will print out `/yolo`, which is the value defined in the current Ubuntu environment, not the Nix environment. The Nix Java package usually configures the `JAVA_HOME` variable to point to the Nix store path, but in Flox that doesn't happen.

Exit the environment.

```bash
exit
```

Now let's try unsetting the `JAVA_HOME` variable in the host environment:

```bash
unset JAVA_HOME
```

Confirm it's empty:

```bash
echo $JAVA_HOME
```

Activate the Flox environment again:

```bash
flox activate
```

Print out the `JAVA_HOME` variable:

```bash
echo $JAVA_HOME
```

The result is an empty string, proving that no matter if the `JAVA_HOME` variable is set or not in the host environment, Flox does not set it automatically, unlike Nix Flakes.
