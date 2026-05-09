# Documentation

This folder contains project documentation for the Smart Fan reverse-engineering and custom controller build.

## Start Here

- If you want the story and context, read [Project Story](project-story.md)
- If you want protocol details, read [UART Protocol](uart-protocol.md)
- If you want hardware layout and assets, read [Hardware Notes](hardware-notes.md)

## Contents

- [Project Story](project-story.md): End-to-end log from teardown to ESPHome integration
- [UART Protocol](uart-protocol.md): Reverse-engineered fan serial protocol and command map
- [Hardware Notes](hardware-notes.md): Board-level findings, connector mapping, and design decisions
- [Schematic PDF](schematic.pdf): Printable schematic export
- [Schematic Image](schematic.png): Quick schematic preview
- [Front Render](front.png): Custom PCB front render
- [Back Render](back.png): Custom PCB back render
- [3D Render](3d.png): Custom PCB 3D view
- [Fan Model](fan-model.png): John Lewis FZ10 17KR fan
- [Fan In Situ](fan-in-situ.jpg): Original installation
- [Control Board Top](control-board-top.jpg): Stock board teardown
- [Control Board Bottom](control-board-bottom.jpg): Stock board connector side

## Scope

These docs focus on:
- Reverse-engineering the fan communication path
- Integrating ESPHome with Home Assistant
- Replacing the stock display/input board with a custom PCB
