# Cover Time Based Component RF (trigger script) version

With this component you can add a time-based cover. You have to set triggering scripts to open, close and stop the cover. Position is calculated based on the fraction of time spent by the cover travelling up or down. You can set position from within Home Assistant using service calls. When you use this component, you can forget about the cover's original remote controllers or switches, because there's no feedback from the cover about its real state, state is assumed based on the last command sent from Home Assistant. There's a custom service available where you can update the real state of the cover based on external sensors if you want to.

[Configuration details and documentation](https://github.com/nagyrobi/home-assistant-custom-components-cover-rf-time-based)


## Supported features:
- Usable with covers which support only triggering, and give no feedback about their state (position is assumed based on commands sent from HA)
- State can be updated based on independent, external sensors (for example a contact or reed sensor at closed or opened state)
- State can mimic the operation based on external sensors (for example by monitoring the air for closing or opening RF codes) so usage in parrallel with controllers outside HA is possible

## Component authors & contributors
    "@davidramosweb",
    "@nagyrobi",
    "@Alfiegerner"

[Support forum](https://community.home-assistant.io/t/custom-component-cover-time-based/187654/3)
