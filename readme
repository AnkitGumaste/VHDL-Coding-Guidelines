## Table of Contents

- [Coding Guidelines](#coding-guidelines)
  - [Module and File Naming](#module-and-file-naming)
  - [Architecture Naming](#architecture-naming)
  - [Source File Header](#source-file-header)
  - [Library and Package Declarations](#library-and-package-declarations)
  - [Port Naming Conventions](#port-naming-conventions)
  - [Formatting](#formatting)
  - [Best Practices](#best-practices)
- [Testbench Conventions](#testbench-conventions)

---

## Coding Guidelines

### Module and File Naming

- The **entity name must match the filename** exactly.
- Both file names and entity names must be in **lowercase only**.


---

### Architecture Naming

The architecture name must follow the pattern `<entity_name>_arch`.

```vhdl
-- Example
entity counter is
  ...
end entity counter;

architecture counter_arch of counter is
  ...
begin
  ...
end architecture counter_arch;
```

---

### Source File Header

Every source file must begin with the following header block.
All fields are mandatory. Line length must not exceed 80 characters.

```vhdl
--=============================================================================
-- Project  : <project name>
-- File     : <file_name.vhd>
-- Author   : <Engineer name>
-- Version  : v0.01
-- Abstract : <Brief one-line description of the module>
--
-- Modification History
--=============================================================================
-- Version | Change Description
--=============================================================================
-- 0.01    | Initial version
--=============================================================================
```

Increment version as `0.01 → 0.02 → ... → 1.00` on significant milestones.
Each history entry must describe the functional change clearly.

---

### Library and Package Declarations

- Always use `ieee.std_logic_1164` and `ieee.numeric_std`.
- **Never** use `std_logic_arith` or `std_logic_unsigned` — these are
  non-standard Synopsys packages and are not IEEE compliant.
- Use `unsigned` / `signed` types from `numeric_std` for arithmetic.

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;
```

---

### Port and Signal Naming Conventions

| Convention   | Type                  | Example              |
|--------------|-----------------------|----------------------|
| `i_` prefix  | Input port            | `i_data`, `i_valid`  |
| `o_` prefix  | Output port           | `o_data`, `o_ready`  |
| `io_` prefix | Bidirectional port    | `io_data`            |
| `s_` prefix  | Internal signal       | `s_count`, `s_state` |
| `_n` suffix  | Active-low signal     | `i_cs_n`, `o_wr_n`   |
| (none)       | Clock — global        | `clk`                |
| (none) + `_n`| Active-low reset      | `rst_n`              |

Rules:
- `i_`, `o_`, `io_` prefixes apply to all ports **except** `clk` and
  `rst_n`, which are global signals and need no directional prefix.
- Active-low signals always carry the `_n` suffix, combined with
  the appropriate prefix where applicable: `i_cs_n`, `o_wr_n`.
- All internal signals use the `s_` prefix — no exceptions.


---

### Formatting

| Rule         | Requirement                                        |
|--------------|----------------------------------------------------|
| Line length  | 80 characters or fewer — enforced consistently     |
| Indentation  | 2 spaces per level                                 |
| Tabs         | Not permitted — spaces only                        |
| Blank lines  | Use to separate logical blocks within a process    |

---

### Best Practices

#### One Process — One Signal

Each process drives exactly **one signal**. This keeps synthesis
predictable, avoids unintended dependencies, and simplifies debugging.


#### Label All Processes

Every process must have a descriptive label in the format
`<signal_name>_proc`. Unlabeled processes are not permitted.

```vhdl
count_proc : process(clk, rst_n)
```

#### Avoid Latches

All combinational processes must cover every output on every branch.
Use a default assignment at the top of the process to guarantee this.


#### Avoid Combinational Feedback

The output of a combinational block must never feed back directly
into its own inputs. Break any feedback path with a registered stage.


#### Signals vs. Variables

- Use **signals** for inter-process communication and module ports.
- Use **variables** only within a process when an intermediate value
  is needed within the same delta cycle.
- Never use variables to communicate between processes.

#### Port Map — Named Association Only

All component instantiations must use named port association.
Positional association is not permitted.


---

## Testbench Conventions

- Testbench files are named `tb_<entity>.vhd` and placed under `tb/`,
  mirroring the `src/` directory structure.
- The testbench entity must be named `tb_<entity>` with no ports.
- The testbench architecture must follow the naming rule:
  `tb_<entity>_arch`.
- Clock generation, stimulus, and response checking must be in
  separate, clearly labeled processes.

```vhdl
--=============================================================================
-- Project  : RTL Library
-- File     : tb_counter.vhd
-- Author   : <Engineer name>
-- Version  : v0.01
-- Abstract : Testbench for counter entity
--
-- Modification History
--=============================================================================
-- Version | Change Description
--=============================================================================
-- 0.01    | Initial version
--=============================================================================

library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity tb_counter is
end entity tb_counter;

architecture tb_counter_arch of tb_counter is

  -- DUT signal declarations
  signal clk   : std_logic := '0';
  signal rst_n : std_logic := '0';

  constant CLK_PERIOD : time := 10 ns;

begin

  -- Clock generation
  clk_proc : process
  begin
    clk <= '0'; wait for CLK_PERIOD / 2;
    clk <= '1'; wait for CLK_PERIOD / 2;
  end process clk_proc;

  -- DUT instantiation
  u_dut : entity work.counter
    port map (
      clk   => clk,
      rst_n => rst_n
    );

  -- Stimulus
  stim_proc : process
  begin
    rst_n <= '0';
    wait for 20 ns;
    rst_n <= '1';
    wait for 100 ns;
    wait;
  end process stim_proc;

end architecture tb_counter_arch;
```

---
