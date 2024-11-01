type: custom:apexcharts-card
all_series_config:
  unit: Cent/kWh
header:
  show: true
  title: Glaskugel
  show_states: true
  colorize_states: true
  standard_format: false
graph_span: 24h
now:
  show: true
  color: 4DD0E1
span:
  start: day
series:
  - entity: sensor.tibber_prices
    name: Durchschnittspreis
    show:
      in_header: before_now
      name_in_header: false
    type: line
    curve: stepline
    extend_to: false
    stroke_width: 2
    float_precision: 2
    data_generator: |
      const noon = new Date()
      noon.setHours(0, 0, 0, 0)
      const prices = entity.attributes.today.concat(entity.attributes.tomorrow);
      const data = [];
      for(let i = 0; i < prices.length; i++) {
        data.push([noon.getTime() + i * 1000 * 3600, prices[i].total * 100])
      }
      return data;
  - entity: sensor.scb_home_power
    name: aktueller Verbrauch
    type: column
    yaxis_id: "1"
    unit: Wh
    color: "#00bfff"
    curve: stepline
apex_config:
  grid:
    show: true
    borderColor: "#E0E0E0"
  chart:
    height: 350px
    type: line
  xaxis:
    type: datetime
    labels:
      datetimeFormatter:
        hour: H:mm
  yaxis:
    - id: "0"
      decimalsInFloat: 0
      title:
        text: Preis (Cent/kWh)
    - id: "1"
      opposite: true
      decimalsInFloat: 0
      title:
        text: Verbrauch (Wh)
