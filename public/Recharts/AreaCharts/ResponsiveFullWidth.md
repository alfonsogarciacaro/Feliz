# Feliz.Recharts - Responsive Area Chart

Charts are usually rendered with a fixed size given a `width` and `height` property but a lot of the times you want the chart to be responsive: resize according to the size of the parent which is what `Recharts.responsiveContainer` is for. Here follows an example of how to use a full width chart.

```fsharp:recharts-area-responsefullwidth
module App

open Feliz
open Feliz.Recharts

type Point = { name: string; uv: int; pv: int }

let data = [
    { name = "Page A"; uv = 4000; pv = 2400 }
    { name = "Page B"; uv = 3000; pv = 1398 }
    { name = "Page C"; uv = 2000; pv = 9800 }
    { name = "Page D"; uv = 2780; pv = 3908 }
    { name = "Page E"; uv = 1890; pv = 4800 }
    { name = "Page F"; uv = 2390; pv = 3800 }
    { name = "Page G"; uv = 3490; pv = 4300 }
]


let createGradient (id: string) color =
    Html.linearGradient [
        prop.id id
        prop.x1 0; prop.x2 0
        prop.y1 0; prop.y2 1
        prop.children [
            Html.stop [
                prop.offset(length.percent 5)
                prop.stopColor color
                prop.stopOpacity 0.8
            ]
            Html.stop [
                prop.offset(length.percent 95)
                prop.stopColor color
                prop.stopOpacity 0.0
            ]
        ]
    ]


let chart = React.functionComponent(fun () ->
    let responsiveChart =
        Recharts.areaChart [
            areaChart.data data
            areaChart.margin(top=10, right=30)
            areaChart.children [
                Html.defs [
                    createGradient "colorUv" "#8884d8"
                    createGradient "colorPv" "#82ca9d"
                ]
                Recharts.xAxis [ xAxis.dataKey (fun point -> point.name) ]
                Recharts.yAxis [ ]
                Recharts.tooltip [ ]
                Recharts.cartesianGrid [ cartesianGrid.strokeDasharray(3, 3) ]
                Recharts.area [
                    area.monotone
                    area.dataKey (fun point -> point.uv)
                    area.stroke "#8884d8"
                    area.fillOpacity 1
                    area.fill "url(#colorUv)"
                ]
                Recharts.area [
                    area.monotone
                    area.dataKey (fun point -> point.pv)
                    area.stroke "#82ca9d"
                    area.fillOpacity 1
                    area.fill "url(#colorPv)"
                ]
            ]
        ]

    Recharts.responsiveContainer [
        responsiveContainer.width (length.percent 100)
        responsiveContainer.height 300
        responsiveContainer.chart responsiveChart
    ])

open Browser.Dom

ReactDOM.render(chart, document.getElementById "root")
```