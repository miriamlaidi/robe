---
icon: lucide/monitor
---
# Class BPMplot
## Graphical interface (NiceGUI) for the BPM simulations
This class builds an input panel (sliders for the beam and the guide parameters, medium selector, injection options) and an output panel with the intensity map $|A(x,z)|^2$ and a status line. The calculation is delegated to the functions of each case (free space, single waveguide, straight coupler, S-bent coupler, Mach-Zehnder), which use the BPM class. 
```mermaid
flowchart LR
    subgraph CLS["class BPMplot"]
        INIT["__init__"] --> BUI["build_ui"]
        BUI --> SL["slider"]
        BUI -.->|"medium, L, h or L_arm change"| SZ["suggest_z"]
        INIT -->|"first plot (timer)"| UPD
        BTN["Simulate button"] --> UPD["update (async)"]
        UPD --> GP["get_params"]
        UPD --> ST["status label"]
    end

    PRED["is_mz, is_s_bend,<br/>is_coupler"] -->|"show or hide sliders"| BUI
    LEN["s_bend_length, mz_length,<br/>DEFAULT_Z"] --> SZ
    GP --> CASE["case function<br/>(free_space, straight_coupler...)"]
    UPD -->|"background thread"| CASE
    CASE --> BPM["class BPM<br/>(propagate.py)"]
    CASE --> DR["draw"]
    DR --> FIG["figure"]
    ADD["add_plot<br/>(ADD PLOT button)"] -->|"creates a new"| INIT
```

## Draw
Before building the class, we define the draw function, which plots the intensity map $|A(x,z)|^2$ from the results of a simulation and which will be used later inside the class.
::: interfaccia.draw
    options:
        show_root_heading: true
        show_root_full_path: false

## Class BPMplot
The class BPMplot only handles the interface: it reads the controls, runs the simulation and displays the result, using the classes listed below.
::: interfaccia.BPMplot

# Extra functions
::: interfaccia.add_plot
    options:
        show_root_heading: true
        show_root_full_path: false
::: interfaccia.remove_all
    options:
            show_root_heading: true
            show_root_full_path: false