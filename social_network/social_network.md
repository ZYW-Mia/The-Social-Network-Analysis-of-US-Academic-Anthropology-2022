<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="echart.js"></script>
    <script src="echarts.min.js"></script>
    <title>The Social Network of US Academic Anthropology</title></title>
</head>
<body>
<div id="water" style="width:100%; height: 800px;"></div>
    <script>
      option = {
    tooltip: {
        show: true, // 默认显示
        showContent: true, // 是否显示提示框浮层
        trigger: 'item', // 触发类型，默认数据项触发
        triggerOn: 'mousemove', // 提示触发条件，mousemove鼠标移至触发，还有click点击触发
        alwaysShowContent: false, // 默认离开提示框区域隐藏，true为一直显示
        showDelay: 100, // 浮层显示的延迟，单位为 ms，默认没有延迟，也不建议设置。在 triggerOn 为 'mousemove' 时有效。
        hideDelay: 2000, // 浮层隐藏的延迟，单位为 ms，在 alwaysShowContent 为 true 的时候无效。
        enterable: false, // 鼠标是否可进入提示框浮层中，默认为false，如需详情内交互，如添加链接，按钮，可设置为 true。
        position: 'right', // 提示框浮层的位置，默认不设置时位置会跟随鼠标的位置。只在 trigger 为'item'的时候有效。
        confine: false, // 是否将 tooltip 框限制在图表的区域内。
        // 外层的 dom 被设置为 'overflow: hidden'，或者移动端窄屏，导致 tooltip 超出外界被截断时，此配置比较有用。
        transitionDuration: 0.2, // 提示框浮层的移动动画过渡时间，单位是秒，设置为 0 的时候会紧跟着鼠标移动。
    },
    legend: { // 图例显示（显示在右上角），name:类别名称，icon:图例的形状（默认是roundRect圆角矩形）。
        show: true,
        data: []
    },
    series: [{
        type: 'graph', // 关系图
        name: 'PhD:', // 系列名称，用于tooltip的显示，legend 的图例筛选，在 setOption 更新数据和配置项时用于指定对应的系列。
        layout: 'circular', // 图的布局，类型为力导图，'circular' 采用环形布局，见示例 Les Miserables
        legendHoverLink: true, // 是否启用图例 hover(悬停) 时的联动高亮。
        hoverAnimation: true, // 是否开启鼠标悬停节点的显示动画
        coordinateSystem: null, // 坐标系可选
        xAxisIndex: 0, // x轴坐标 有多种坐标系轴坐标选项
        yAxisIndex: 0, // y轴坐标 
        circular: {
                rotateLabel: true,
            },
        force: { // 力引导图基本配置
            // initLayout: , // 力引导的初始化布局，默认使用xy轴的标点
            repulsion: 500, // 节点之间的斥力因子。支持数组表达斥力范围，值越大斥力越大。
            gravity: 0.02, // 节点受到的向中心的引力因子。该值越大节点越往中心点靠拢。
            edgeLength: 100, // 边的两个节点之间的距离，这个距离也会受 repulsion影响 。值越大则长度越长
            layoutAnimation: true // 因为力引导布局会在多次迭代后才会稳定，这个参数决定是否显示布局的迭代动画
            // 在浏览器端节点数据较多（>100）的时候不建议关闭，布局过程会造成浏览器假死。                        
        },
        roam: true, // 是否开启鼠标缩放和平移漫游。默认不开启，true 为都开启。如果只想要开启缩放或者平移，可以设置成 'scale' 或者 'move'
        nodeScaleRatio: 0.6, // 鼠标漫游缩放时节点的相应缩放比例，当设为0时节点不随着鼠标的缩放而缩放
        draggable: true, // 节点是否可拖拽，只在使用力引导布局的时候有用。
        focusNodeAdjacency: true, // 是否在鼠标移到节点上的时候突出显示节点以及节点的边和邻接节点。
        // symbol:'roundRect', // 关系图节点标记的图形。
        // ECharts 提供的标记类型包括：'circle'(圆形), 'rect'（矩形）, 'roundRect'（圆角矩形）,
        // 'triangle'（三角形）, 'diamond'（菱形）, 'pin'（大头针）, 'arrow'（箭头）
        // 也可以通过 'image://url' 设置为图片，其中 url 为图片的链接。'path:// 这种方式可以任意改变颜色并且抗锯齿
        // symbolSize: 10 , // 也可以用数组分开表示宽和高，例如 [20, 10] 如果需要每个数据的图形大小不一样，
        // 可以设置为如下格式的回调函数：(value: Array|number, params: Object) => number|Array
        // symbolRotate:, // 关系图节点标记的旋转角度。注意在 markLine 中当 symbol 为 'arrow' 时会忽略 symbolRotate 强制设置为切线的角度。
        // symbolOffset:[0,0], // 关系图节点标记相对于原本位置的偏移。[0, '50%']
        edgeSymbol: ['none', 'none'], // 边两端的标记类型，可以是一个数组分别指定两端，也可以是单个统一指定。
        // 默认不显示标记，常见的可以设置为箭头，如下：edgeSymbol: ['circle', 'arrow']
        edgeSymbolSize: 10, // 边两端的标记大小，可以是一个数组分别指定两端，也可以是单个统一指定。

        itemStyle: { // ========图形样式，有 normal 和 emphasis 两个状态。
            // normal 是图形在默认状态下的样式；
            // emphasis 是图形在高亮状态下的样式，比如在鼠标悬浮或者图例联动高亮时。
            normal: { // 默认样式
                label: {
                    show: true
                },
                borderType: 'solid', // 图形描边类型，默认为实线，支持 'solid'（实线）, 'dashed'(虚线), 'dotted'（点线）。
                borderColor: 'rgba(205, 149, 12, 0.4)', // 设置图形边框为淡金色,透明度为0.4
                borderWidth: 2, // 图形的描边线宽。为 0 时无描边。
                opacity: 1 // 图形透明度。支持从 0 到 1 的数字，为 0 时不绘制该图形。默认0.5

            },
            emphasis: { // 高亮状态

            }
        },
        lineStyle: { // ========关系边的公用线条样式。
            normal: {
                color: '#a0a39d',
                width: '1', //线的粗细
                type: 'solid', // 线的类型 'solid'（实线）'dashed'（虚线）'dotted'（点线）
                curveness: 0.4, // 线条的曲线程度，从0到1
                opacity: 0.5 // 图形透明度。支持从 0 到 1 的数字，为 0 时不绘制该图形。默认0.5
            },
            emphasis: { // 高亮状态

            }
        },
        label: { // ========结点图形上的文本标签
            normal: {
                show: true, // 是否显示标签。
                position: 'inside', // 标签的位置。['50%', '50%'] [x,y]
                textStyle: { // 标签的字体样式
                    color: '#d3d7d4', // 字体颜色 #cde6c7 #d1c7b7 #d9d6c3 #d3d7d4
                    fontStyle: 'normal', // 文字字体的风格 'normal'标准 'italic'斜体 'oblique' 倾斜
                    fontWeight: 'bolder', // 'normal'标准，'bold'粗的，'bolder'更粗的，'lighter'更细的，或100 | 200 | 300 | 400...
                    fontFamily: 'sans-serif', // 文字的字体系列
                    fontSize: 12, // 字体大小
                }
            },
            emphasis: { // 高亮状态

            }
        },
        edgeLabel: { // ========连接线上的文本标签 
            normal: {
                show: false // 不显示连接线上的文字，如果显示只能显示结点的value值，而不是连接线的值
            },
            emphasis: { // 高亮状态

            }
        },
        categories: [ // name(类别名称)要同legend(图例）按次序一致
            {
                name: '色彩1'
            },
            {
                name: '色彩2'
            },
            {
                name: '色彩3'
            },
            {
                name: '色彩4'
            },
        ],
        // 设置结点node的数据
        // category: 类别序号，从0开始
        // name: 结点图形显示的文字
        // value: 鼠标点击结点，显示的数字
        // symbolSize: 结点图形的大小
        // symbol: 类目节点标记图形，默然为圆形
        // label: 标签样式
        data: [{
                category: '色彩3',
                name: 'Arizona',
                value: 64,
                symbolSize: 64
            },
            {
                category: '色彩3',
                name: 'Arizona State',
                value: 25,
                symbolSize: 25
            },
            {
                category: '色彩3',
                name: 'Boston University',
                value: 6,
                symbolSize: 6
            },
            {
                category: '色彩3',
                name: 'Brandies',
                value: 5,
                symbolSize: 5
            },
            {
                category: '色彩1',
                name: 'UC Berkeley',
                value: 112,
                symbolSize: 112
            },
            {
                category: '色彩1',
                name: 'Brown',
                value: 14,
                symbolSize: 14
            },
            {
                category: '色彩1',
                name: 'Chicago',
                value: 139,
                symbolSize: 139
            },
            {
              category: '色彩2',
                name: 'Colorado',
                value: 12,
                symbolSize: 12
            },
            {
                category: '色彩3',
                name: 'Columbia',
                value: 44,
                symbolSize: 44
            },
            {
                category: '色彩1',
                name: 'Cornell',
                value: 24,
                symbolSize: 24
            },
            {
               category: '色彩4',
                name: 'CUNY',
                value: 42,
                symbolSize: 42
            },
            {
               category: '色彩4',
                name: 'Duke',
                value: 24,
                symbolSize: 24
            },
            {
                category: '色彩3',
                name: 'Emory',
                value: 21,
                symbolSize: 21
            },
            {
                category: '色彩3',
                name: 'Florida',
                value: 35,
                symbolSize: 35
            },
            {
                category: '色彩1',
                name: 'Georgia',
                value: 14,
                symbolSize: 14
            },
            {
                category: '色彩3',
                name: 'GWU',
                value:5,
                symbolSize: 5
            },
            {
                category: '色彩1',
                name: 'Harvard',
                value: 112,
                symbolSize: 112
            },
            {
                category: '色彩3',
                name: 'Hawaii',
                value: 9,
                symbolSize: 9
            },
            {
                category: '色彩1',
                name: 'IUB',
                value: 23,
                symbolSize: 23
            },
            {
              category: '色彩2',
                name: 'JHU',
                value: 15,
                symbolSize: 15
            },
            {
                category: '色彩1',
                name: 'Kentucky',
                value: 8,
                symbolSize: 8
            },
            {
                category: '色彩3',
                name: 'Massachusetts',
                value: 18,
                symbolSize: 18
            },
            {
                category: '色彩1',
                name: 'Michigan',
                value: 108,
                symbolSize: 108
            },
            {
               category: '色彩4',
                name: 'Michigan State',
                value: 7,
                symbolSize: 7
            },
            {
               category: '色彩4',
                name: 'Minnesota',
                value: 13,
                symbolSize: 13
            },
            {
                category: '色彩1',
                name: 'Missouri',
                value: 9,
                symbolSize: 9
            },
            {
                category: '色彩1',
                name: 'MIT',
                value: 10,
                symbolSize: 10
            },
            {
                category: '色彩3',
                name: 'New School',
                value: 5,
                symbolSize: 5
            },
            {
                category: '色彩3',
                name: 'New Mexico',
                value: 37,
                symbolSize: 37
            },
            {
                category: '色彩3',
                name: 'Northwestern',
                value: 24,
                symbolSize: 24
            },
            {
                category: '色彩1',
                name: 'NYU',
                value: 49,
                symbolSize: 49
            },
            {
                category: '色彩3',
                name: 'Ohio State',
                value: 8,
                symbolSize: 8
            },
            {
                category: '色彩1',
                name: 'Pennsylvania',
                value: 47,
                symbolSize: 47
            },
            {
                category: '色彩1',
                name: 'Pennsylvania State',
                value: 29,
                symbolSize: 29
            },
            {
                category: '色彩3', 
                name: 'Pittsburgh',
                value: 12,
                symbolSize: 12
            },
            {
                category: '色彩3',
                name: 'Princeton',
                value: 20,
                symbolSize: 20
            },
            {
                category: '色彩1',
                name: 'Rice',
                value: 7,
                symbolSize: 7
            },
            {
                category: '色彩1',
                name: 'Rutgers',
                value: 15,
                symbolSize: 15
            },
            {
                category: '色彩1',
                name: 'South Florida',
                value: 6,
                symbolSize: 6
            },
            {
               category: '色彩4',
                name: 'Southern Methodist',
                value: 9,
                symbolSize: 9
            },
            {
              category: '色彩2',
                name: 'Stanford',
                value: 55,
                symbolSize: 55
            },
            {
                category: '色彩3',
                name: 'SUNY Albany',
                value: 9,
                symbolSize: 9
            },
            {
                category: '色彩1',
                name: 'SUNY Binghamton',
                value: 11,
                symbolSize: 11
            },
        
            {
                category: '色彩3',
                name: 'SUNY Buffalo',
                value: 5,
                symbolSize: 5
            },
            {
                category: '色彩3',
                name: 'SUNY Stony Brook',
                value: 23,
                symbolSize: 23
            },
            {
                category: '色彩1',
                name: 'Syracuse',
                value: 7,
                symbolSize: 7
            },
            {
                category: '色彩1',
                name: 'Tulane',
                value: 12,
                symbolSize: 12
            },
            {
                category: '色彩1',
                name: 'TAMU',
                value: 11,
                symbolSize: 11
            },
            {
                category: '色彩1',
                name: 'Tennessee',
                value: 11,
                symbolSize: 11
            },
            {
              category: '色彩2',
                name: 'UC Irvine',
                value: 5,
                symbolSize: 5
            },
            {
              category: '色彩2',
                name: 'Virginia',
                value: 8,
                symbolSize: 8
            },
            {
                category: '色彩1',
                name: 'UC San Diego',
                value: 23,
                symbolSize: 23
            },
            
            {
                category: '色彩2',
                name: 'UC Davis',
                value: 29,
                symbolSize: 29
            },
            {
                category: '色彩1',
                name: 'UCLA',
                value: 47,
                symbolSize: 47
            },
            {
                category: '色彩3',
                name: 'UIUC',
                value: 30,
                symbolSize: 30
            },
            {
              category: '色彩2',
                name: 'UNC',
                value: 24,
                symbolSize: 24
            },
            {
                category: '色彩1',
                name: 'Utah',
                value: 5,
                symbolSize: 5
            },
            {
                category: '色彩1',
                name: 'Vanderbilt',
                value: 10,
                symbolSize: 10
            },
            {
                category: '色彩3',
                name: 'UT Austin',
                value: 43,
                symbolSize: 43
            },
            {
                category: '色彩3',
                name: 'Washington',
                value: 41,
                symbolSize: 41
            },
            {
              category: '色彩2',
                name: 'Wisconsin Madison',
                value: 23,
                symbolSize: 23
            },
            {
                category: '色彩3',
                name: 'WUSTL',
                value: 25,
                symbolSize: 25
            },
            {
                category: '色彩1',
                name: 'Yale',
                value: 55,
                symbolSize: 55
            }
        ],
        links: [ // 设置连线edges的数据
            {
                target: 'Arizona',
                source: 'Colorado',
                value: 1
                
                },
            {
                target: 'Arizona',
                source: 'Duke',
                
            },
            {
                target: 'Arizona',
                source: 'Harvard',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Kentucky',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Michigan',
                value: 2
                
            },
            {
                target: 'Arizona',
                source: 'New Mexico',
                value: 4
                
            },
            {
                target: 'Arizona',
                source: 'Pennsylvania',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Southern Methodist',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Stanford',
                value: 3
              
            },
            {
                target: 'Arizona',
                source: 'UC Berkeley',
                value: 4
            
            },
            {
                target: 'Arizona',
                source: 'UC Davis',
                value: 1
               
            },

            {
                target: 'Arizona',
                source: 'UT Austin',
                value: 1
               
            },
            {
                target: 'Arizona',
                source: 'Vanderbilt',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Wisconsin Madison',
                value: 1
                
            },
            {
                target: 'Arizona',
                source: 'Yale',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Arizona',
                value: 2
               
            },
            {
                target: 'Arizona State',
                source: 'Chicago',
                value: 3
                
            },
            {
                target: 'Arizona State',
                source: 'Cornell',
                value: 2
                
            },
            {
                target: 'Arizona State',
                source: 'Emory',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Florida',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'GWU',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'IUB',
                value: 1
               
            },
            {
                target: 'Arizona State',
                source: 'JHU',
                value: 1
               
            },
            {
                target: 'Arizona State',
                source: 'Massachusetts',
                value: 1
               
            },
            {
                target: 'Arizona State',
                source: 'Michigan',
                value: 7
              
            },
            {
                target: 'Arizona State',
                source: 'Minnesota',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'New Mexico',
                value: 1
               
            },
            {
                target: 'Arizona State',
                source: 'Northwestern',
                value: 1
              
            },
            {
                target: 'Arizona State',
                source: 'NYU',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Pennsylvania State',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Rutgers',
                value: 1
              
            },
            {
                target: 'Arizona State',
                source: 'SUNY Albany',
                value: 1
              
            },
            {
                target: 'Arizona State',
                source: 'SUNY Stony Brook',
                value: 1
              
            },
            {
                target: 'Arizona State',
                source: 'UC Berkeley',
                value: 2
                
            },
            {
                target: 'Arizona State',
                source: 'UC Davis',
                value: 4
                
            },
            {
                target: 'Arizona State',
                source: 'UC San Diego',
                value: 1
               
            },
            {
                target: 'Arizona State',
                source: 'UCLA',
                value: 3
               
            },
            {
                target: 'Arizona State',
                source: 'UIUC',
                value: 2
                
            },
            {
                target: 'Arizona State',
                source: 'UNC',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Utah',
                value: 2
                
            },
            {
                target: 'Arizona State',
                source: 'Vanderbilt',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Washington',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'Wisconsin Madison',
                value: 1
                
            },
            {
                target: 'Arizona State',
                source: 'WUSTL',
                value: 1
              
            },
            {
                target: 'Arizona State',
                source: 'CUNY',
                value: 1
               
            },
            {
                target: 'Boston University',
                source: 'Chicago',
                value: 2
               
            },
            {
                target: 'Boston University',
                source: 'CUNY',
                value: 1
               
            },
            {
                target: 'Boston University',
                source: 'Emory',
                value: 1
               
            },
            {
                target: 'Boston University',
                source: 'Harvard',
                value: 7
                
            },
            {
                target: 'Boston University',
                source: 'IUB',
                value: 1
                
            },
            {
                target: 'Boston University',
                source: 'JHU',
                value: 1
                
            },
            {
                target: 'Boston University',
                source: 'Michigan',
                value: 2
               
            },
            {
                target: 'Boston University',
                source: 'MIT',
                value: 1
                
            },
            {
                target: 'Boston University',
                source: 'NYU',
                value: 1
                
            },
            {
                target: 'Boston University',
                source: 'Pennsylvania',
                value: 1
                
            },
            {
                target: 'Boston University',
                source: 'UCLA',
                value: 4
               
            },
            {
                target: 'Boston University',
                source: 'UIUC',
                value: 1
                
            },
            {
                target: 'Brandies',
                source: 'Brown',
                value: 1
                
            },
            {
                target: 'Brandies',
                source: 'Chicago',
                value: 4
                
            },
            {
                target: 'Brandies',
                source: 'Michigan',
                value: 2
              
            },
            {
                target: 'Brandies',
                source: 'Pennsylvania',
                value: 1
              
            },
            {
                target: 'Brandies',
                source: 'UC Berkeley',
                value: 3
              
            },
            {
                target: 'Brandies',
                source: 'Yale',
                value: 1
                
            },
            {
                target: 'Brown',
                source: 'Brandies',
                value: 2
                
            },
            {
                target: 'Brown',
                source: 'Chicago',
                value: 1
               
            },
            {
                target: 'Brown',
                source: 'Harvard',
                value: 6
              
            },
            {
                target: 'Brown',
                source: 'Michigan',
                value: 2
              
            },
            {
                target: 'Brown',
                source: 'Minnesota',
                value: 1
                
            },
            {
                target: 'Brown',
                source: 'NYU',
                value: 1
              
            },
            {
                target: 'Brown',
                source: 'SUNY Binghamton',
                value: 1
                
            },
            {
                target: 'Brown',
                source: 'TAMU',
                value: 2
               
            },
            {
                target: 'Brown',
                source: 'UC Berkeley',
                value: 1
              
            },
            {
                target: 'Brown',
                source: 'Yale',
                value: 1
          
            },
            {
                target: 'Brown',
                source: 'UCLA',
                value: 1
               
            },
            
            {
                target: 'SUNY Albany',
                source: 'Colorado',
                value: 2
               
            },
            {
                target: 'SUNY Albany',
                source: 'CUNY',
                value: 1
                
            },
            {
                target: 'SUNY Albany',
                source: 'Florida',
                value: 3
                
            },
            {
                target: 'SUNY Albany',
                source: 'Missouri',
                value: 2
                
            },
            {
                target: 'SUNY Albany',
                source: 'Pennsylvania',
                value: 5
                
            },
            {
                target: 'SUNY Albany',
                source: 'Pennsylvania State',
                value: 2
              
            },
            {
                target: 'SUNY Albany',
                source: 'SUNY Albany',
                value: 2
               
            },
            {
                target: 'SUNY Albany',
                source: 'SUNY Buffalo',
                value: 2
               
            },
            {
                target: 'Chicago',
                source: 'Brown',
                value: 1
                
            },
            {
                target: 'Chicago',
                source: 'Columbia',
                value: 2
               
            },
            {
                target: 'Chicago',
                source: 'Harvard',
                value: 2
                
            },
            {
                target: 'Chicago',
                source: 'JHU',
                value: 1

            },
            {
                target: 'Chicago',
                source: 'Michigan',
                value: 3
              
            },
            {
                target: 'Chicago',
                source: 'MIT',
                value: 1

            },
            {
                target: 'Chicago',
                source: 'NYU',
                value: 2
               
            },
            {
                target: 'Chicago',
                source: 'Pennsylvania',
                value: 1

            },
            {
                target: 'Chicago',
                source: 'Stanford',
                value: 1
              
            },
            {
                target: 'Chicago',
                source: 'Syracuse',
                value: 1
               
            },
            {
                target: 'Chicago',
                source: 'UC Berkeley',
                value: 5
             
            },
            {
                target: 'Chicago',
                source: 'UC San Diego',
                value: 1
                
            },
            {
                target: 'Chicago',
                source: 'Yale',
                value: 1
               
            },
            {
                target: 'Colorado',
                source: 'Arizona',
                value: 1
               
            },
            {
                target: 'Colorado',
                source: 'Arizona State',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Chicago',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Cornell',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Duke',
                value: 1
               
            },
            {
                target: 'Colorado',
                source: 'Michigan',
                value: 1
            
            },
            {
                target: 'Colorado',
                source: 'New Mexico',
                value: 1
              
            },
            {
                target: 'Colorado',
                source: 'Northwestern',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'NYU',
                value: 1
               
            },
            {
                target: 'Colorado',
                source: 'Pennsylvania',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Pennsylvania State',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Rutgers',
                value: 2
               
            },
            {
                target: 'Colorado',
                source: 'SUNY Binghamton',
                value: 1
                
            },
            {
                target: 'Colorado',
                source: 'Syracuse',
                value: 1
              
            },
            {
                target: 'Colorado',
                source: 'UC Berkeley',
                value: 1
             
            },

            {
                target: 'Colorado',
                source: 'UIUC',
                value: 1

            },
            {
                target: 'Colorado',
                source: 'Washington',
                value: 1
               
            },
            {
                target: 'Columbia',
                source: 'Chicago',
                value: 4
               
            },
            {
                target: 'Columbia',
                source: 'Cornell',
                value: 2
             
            },
            {
                target: 'Columbia',
                source: 'Duke',
                value: 1
        
            },
            {
                target: 'Columbia',
                source: 'Harvard',
                value: 3
            },
            {
                target: 'Columbia',
                source: 'New School',
                value: 1
            },
            {
                target: 'Columbia',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'Columbia',
                source: 'NYU',
                value: 3
            },
            {
                target: 'Columbia',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'Columbia',
                source: 'Princeton',
                value: 1
           
            },
            {
                target: 'Columbia',
                source: 'Rutgers',
                value: 1
              
            },
            {
                target: 'Columbia',
                source: 'Stanford',
                value: 1
               
            },
            {
                target: 'Columbia',
                source: 'UC Berkeley',
                value: 5

            },
            {
                target: 'Columbia',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Columbia',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Cornell',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Cornell',
                source: 'Chicago',
                value: 3
            },
            {
                target: 'Cornell',
                source: 'Columbia',
                value: 2
                
            },
            {
                target: 'Cornell',
                source: 'GWU',
                value: 1
                
            },
            {
                target: 'Cornell',
                source: 'JHU',
                value: 1
                
            },
            {
                target: 'Cornell',
                source: 'Michigan',
                value: 2
             
            },
            {
                target: 'Cornell',
                source: 'New School',
                value: 1
               
            },
            {
                target: 'Cornell',
                source: 'NYU',
                value: 2
               
            },
            {
                target: 'Cornell',
                source: 'SUNY Stony Brook',
                value: 1
                
            },
            {
                target: 'Cornell',
                source: 'UC Berkeley',
                value: 3
             
            },
            {
                target: 'Cornell',
                source: 'UC San Diego',
                value: 1
               
            },
            {
                target: 'Cornell',
                source: 'UNC',
                value: 2
               
            },
            {
                target: 'Cornell',
                source: 'Vanderbilt',
                value: 1
                
            },
            {
                target: 'Cornell',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'Cornell',
                source: 'Yale',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Chicago',
                value: 12
            },
            {
                target: 'CUNY',
                source: 'Columbia',
                value: 6
            },
            {
                target: 'CUNY',
                source: 'CUNY',
                value: 7
            },
            {
                target: 'CUNY',
                source: 'Duke',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Harvard',
                value: 6
            },
            {
                target: 'CUNY',
                source: 'Michigan',
                value: 6
            },
            {
                target: 'CUNY',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'NYU',
                value: 6
            },
            {
                target: 'CUNY',
                source: 'Ohio State',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'SUNY Stony Brook',
                value: 2
            },
            {
                target: 'CUNY',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'CUNY',
                source: 'UC Berkeley',
                value: 5
            },
            {
                target: 'CUNY',
                source: 'Yale',
                value: 4
            },
            {
                target: 'Duke',
                source: 'Brown',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Chicago',
                value: 6
            },
            {
                target: 'Duke',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Duke',
                source: 'CUNY',
                value: 8
            },
            {
                target: 'Duke',
                source: 'Duke',
                value: 4
            },
            {
                target: 'Duke',
                source: 'Emory',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'Duke',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Stanford',
                value: 4
            },
            {
                target: 'Duke',
                source: 'SUNY Binghamton',
                value: 1
            },
            {
                target: 'Duke',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'Duke',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'Duke',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Duke',
                source: 'UT Austin',
                value: 2
            },
            {
                target: 'Duke',
                source: 'Virginia',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Washington',
                value: 1
            },
            {
                target: 'Duke',
                source: 'Yale',
                value: 2
            },
            {
                target: 'Emory',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Emory',
                source: 'Columbia',
                value: 2
            },
            {
                target: 'Emory',
                source: 'Emory',
                value: 1
            },
            {
                target: 'Emory',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'Emory',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Emory',
                source: 'IUB',
                value: 1
            },
            {
                target: 'Emory',
                source: 'Michigan',
                value: 3
            },
            {
                target: 'Emory',
                source: 'Minnesota',
                value: 1
            },
            {
                target: 'Emory',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Emory',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'Emory',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Florida',
                value: 4
            },
            {
                target: 'Florida',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'Florida',
                source: 'JHU',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Massachusetts',
                value: 1
            },
            {
                target: 'Florida',
                source: 'New School',
                value: 1
            },
            {
                target: 'Florida',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'Florida',
                source: 'Pittsburgh',
                value: 2
            },
            {
                target: 'Florida',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Florida',
                source: 'SUNY Albany',
                value: 1
            },
            {
                target: 'Florida',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'Florida',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'Florida',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Florida',
                source: 'WUSTL',
                value: 2
            },
            {
                target: 'Florida',
                source: 'Yale',
                value: 2
            },
            {
                target: 'Georgia',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'Kentucky',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'Georgia',
                source: 'Missouri',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'Ohio State',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'Pennsylvania State',
                value: 1
            },

            {
                target: 'Georgia',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Georgia',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Chicago',
                value: 6
            },
            {
                target: 'Harvard',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Duke',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Harvard',
                value: 4
            },
            {
                target: 'Harvard',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Pennsylvania',
                value: 2
            },
            {
                target: 'Harvard',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'Harvard',
                source: 'Virginia',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Brown',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Rutgers',
                value: 1
            },
            {
                target: 'Hawaii',
                source: 'Yale',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Brandeis',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Chicago',
                value: 6
            },
            {
                target: 'IUB',
                source: 'Cornell',
                value: 2
            },
            {
                target: 'IUB',
                source: 'Duke',
                value: 1
            },
 
            {
                target: 'IUB',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'IUB',
                source: 'MIT',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Northwestern',
                value: 3
            },
            {
                target: 'IUB',
                source: 'UC Berkeley',
                value: 3
            },
            {
                target: 'IUB',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'IUB',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'IUB',
                source: 'UIUC',
                value: 2
            },
            {
                target: 'IUB',
                source: 'Vanderbilt',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Washington',
                value: 1
            },
            {
                target: 'IUB',
                source: 'Yale',
                value: 1
            },
            {
                target: 'JHU',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'JHU',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'JHU',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'JHU',
                source: 'MIT',
                value: 2
            },
            {
                target: 'JHU',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'JHU',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Boston University',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Emory',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'IUB',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Kentucky',
                value: 2
            },
            {
                target: 'Kentucky',
                source: 'Massachusetts',
                value: 2
            },
            {
                target: 'Kentucky',
                source: 'New School',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Ohio State',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'SUNY Albany',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'SUNY Binghamton',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Tulane',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'UC Berkeley',
                value: 3
            },
            {
                target: 'Kentucky',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Washington',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'Washington State',
                value: 1
            },
            {
                target: 'Kentucky',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Arizona',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'Chicago',
                value: 8
            },
            {
                target: 'Michigan',
                source: 'Duke',
                value: 3
            },
            {
                target: 'Michigan',
                source: 'Emory',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Harvard',
                value: 6
            },
            {
                target: 'Michigan',
                source: 'JHU',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Michigan',
                value: 4
            },
            {
                target: 'Michigan',
                source: 'Northwestern',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Pennsylvania',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Princeton',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'UNC',
                value: 2
            },
            {
                target: 'Michigan',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Washington',
                value:1
            },
            {
                target: 'Michigan',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'Michigan',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'IUB',
                value: 2
            },
            {
                target: 'Michigan State',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'Michigan State',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'Washington',
                value: 1
            },
            {
                target: 'Michigan State',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'Minnesota',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Harvard',
                value: 4
            },
            {
                target: 'Minnesota',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'JHU',
                value: 2
            },
            {
                target: 'Minnesota',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Minnesota',
                source: 'UC San Diego',
                value: 1
            },
            {
                target: 'Missouri',
                source: 'Colorado',
                value: 1
            },
            {
                target: 'Missouri',
                source: 'IUB',
                value: 1
            },
            {
                target: 'Missouri',
                source: 'New Mexico',
                value: 3
            },
            {
                target: 'Missouri',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Missouri',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'Missouri',
                source: 'Washington',
                value: 1
            },
            {
                target: 'MIT',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'MIT',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'MIT',
                source: 'JHU',
                value: 1
            },
            {
                target: 'MIT',
                source: 'MIT',
                value: 1
            },
            {
                target: 'MIT',
                source: 'NYU',
                value: 2
            },
            {
                target: 'MIT',
                source: 'Pennsylvania',
                value: 2
            },
            {
                target: 'MIT',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'MIT',
                source: 'Stanford',
                value: 3
            },
            {
                target: 'MIT',
                source: 'UC Berkeley',
                value: 1
            },
            
            {
                target: 'New School',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'New School',
                source: 'NYU',
                value: 1
            },
            {
                target: 'New School',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'New School',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'New School',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Emory',
                value: 3
            },
            {
                target: 'Northwestern',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'Northwestern',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'Northwestern',
                source: 'NYU',
                value: 2
            },
            {
                target: 'Northwestern',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'Northwestern',
                source: 'UIUC',
                value: 2
            },
           {
                target: 'NYU',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'NYU',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'NYU',
                source: 'CUNY',
                value: 3
            },
            {
                target: 'NYU',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'NYU',
                source: 'Pennsylvania',
                value: 3
            },
            {
                target: 'NYU',
                source: 'Rice',
                value: 1
            },
            {
                target: 'NYU',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'NYU',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'NYU',
                source: 'Yale',
                value: 2
            },
            {
                target: 'Ohio State',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Colorado',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Hawaii',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'IUB',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Michigan State',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Minnesota',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Oregon',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Wisconsin Madison',
                value: 2
            },
            {
                target: 'Ohio State',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'Ohio State',
                source: 'Yale',
                value: 1
            },
           
            {
                target: 'Pennsylvania',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'Pennsylvania',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Emory',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Massachusetts',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'Pennsylvania',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Pennsylvania',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Brown',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Northwestern',
                value: 2
            },
            {
                target: 'Pennsylvania State',
                source: 'Pennsylvania State',
                value: 3
            },
            {
                target: 'Pennsylvania State',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'UC Davis',
                value: 2
            },
            {
                target: 'Pennsylvania State',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Washington',
                value: 2
            },
            {
                target: 'Pennsylvania State',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'Pennsylvania State',
                source: 'Yale',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'Duke',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'Pittsburgh',
                source: 'MIT',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'Pittsburgh',
                source: 'UC Santa Cruz',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Pittsburgh',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Princeton',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'Princeton',
                source: 'Columbia',
                value: 2
            },
            {
                target: 'Princeton',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'Princeton',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'Princeton',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'Princeton',
                source: 'UC Berkeley',
                value: 4
            },
            {
                target: 'Princeton',
                source: 'UCLA',
                value: 2
            },
            {
                target: 'Princeton',
                source: 'UCLA',
                value: 2
            },
            {
                target: 'Princeton',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Rice',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Rice',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Rice',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'Rice',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'Rice',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Rice',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Rice',
                source: 'UC Irvine',
                value: 1
            },
            {
                target: 'Rice',
                source: 'UC Santa Barbara',
                value: 1
            },
            {
                target: 'Rice',
                source: 'Virginia',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'Rutgers',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'UC Berkeley',
                value: 3
            },
            {
                target: 'Rutgers',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Rutgers',
                source: 'Utah',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Arizona',
                value: 3
            },
            {
                target: 'South Florida',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Emory',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Florida',
                value: 3
            },
            {
                target: 'South Florida',
                source: 'Georgia',
                value: 2
            },
            {
                target: 'Southern Methodist',
                source: 'Arizona',
                value: 3
            },
            {
                target: 'South Florida',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'South Florida',
                source: 'Hawaii',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Minnesota',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'NYU',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'NYU',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Rutgers',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'NYU',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'South Florida',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'SUNY Albany',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Tennessee',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'South Florida',
                source: 'Virginia',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Arizona State',
                value: 3
            },
            {
                target: 'Southern Methodist',
                source: 'Brown',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'Southern Methodist',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'Southern Methodist',
                source: 'NYU',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Rutgers',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'South Florida',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Southern Methodist',
                source: 'UCLA',
                value: 2
            },
            {
                target: 'Southern Methodist',
                source: 'Washington',
                value: 1
            },

            {
                target: 'Stanford',
                source: 'Chicago',
                value: 4
            },
            {
                target: 'Stanford',
                source: 'Harvard',
                value: 4
            },
            {
                target: 'Stanford',
                source: 'UC Santa Cruz',
                value: 1
            },
            {
                target: 'Stanford',
                source: 'Massachusetts',
                value: 1
            },
            {
                target: 'Stanford',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'Stanford',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'Stanford',
                source: 'Washington',
                value: 1
            },
            {
                target: 'Stanford',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'New School',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'NYU',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'SUNY Albany',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'SUNY Albany',
                source: 'UT Austin',
                value: 2
            },
            {
                target: 'SUNY Albany',
                source: 'Yale',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Arizona',
                value: 2
            },
            {
                target: 'SUNY Binghamton',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'CUNY',
                value: 2
            },
            {
                target: 'SUNY Binghamton',
                source: 'Hawaii',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Minnesota',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Minnesota',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'NYU',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Pittsburgh',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Tennessee',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'SUNY Binghamton',
                source: 'Washington',
                value: 2
            },
            {
                target: 'SUNY Binghamton',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'Brandeis',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'JHU',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'SUNY Buffalo',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'SUNY Stony Brook',
                value: 2
            },
            {
                target: 'SUNY Stony Brook',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'Yale',
                value: 1
            },
            {
                target: 'SUNY Stony Brook',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'Florida',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'Rutgers',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'SUNY Binghamton',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'Syracuse',
                source: 'UC Santa Barbara',
                value: 2
            },
            {
                target: 'Syracuse',
                source: 'UCLA',
                value: 3
            },
            {
                target: 'Syracuse',
                source: 'Utah',
                value: 1
            },
            
            {
                target: 'TAMU',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'IUB',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'Florida',
                value: 1
            },
       
            {
                target: 'TAMU',
                source: ' New Mexico',
                value: 2
            },
            {
                target: 'TAMU',
                source: 'NYU',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'UC Santa Barbara',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'TAMU',
                source: 'WUSTL',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Duke',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Florida',
                value: 2
            },
            {
                target: 'Tennessee',
                source: 'JHU',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Michigan',
                value: 3
            },
            {
                target: 'Tennessee',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'SUNY Albany',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'SUNY Binghamton',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Tennessee',
                value: 3
            },
           
            {
                target: 'Tennessee',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'UT Austin',
                value: 2
            },
            {
                target: 'Tennessee',
                source: 'Washington',
                value: 1
            },
            {
                target: 'Tennessee',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'Tulane',
                source: 'Columbia',
                value: 2
            },
            {
                target: 'Tulane',
                source: 'Cornell',
                value: 2
            },
            {
                target: 'Tulane',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Hawaii',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Tulane',
                value: 2
            },
            {
                target: 'Tulane',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'UNC',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'Tulane',
                source: 'Yale',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Chicago',
                value: 4
            },
            {
                target: 'UC Berkeley',
                source: 'Columbia',
                value: 2
            },
            {
                target: 'UC Berkeley',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'UC Berkeley',
                source: 'JHU',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'NYU',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'Chicago',
                value: 1
            },
            
            {
                target: 'UC Berkeley',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'UC Berkeley',
                source: 'Yale',
                value: 2
            },
            {
                target: 'UC Irvine',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'Harvard',
                value: 3
            },
            {
                target: 'UC Irvine',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'NYU',
                value: 2
            },
            {
                target: 'UC Irvine',
                source: 'Rice',
                value: 3
            },
            {
                target: 'UC Irvine',
                source: 'Stanford',
                value: 4
            },
            {
                target: 'UC Irvine',
                source: 'UC Berkeley',
                value: 4
            },
            {
                target: 'UC Irvine',
                source: 'UC San Diego',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'UC Irvine',
                source: 'Yale',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'UC Davis',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Missouri',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Stanford',
                value: 4
            },
            {
                target: 'UC Davis',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'UC Davis',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'UC Santa Barbara',
                value: 1
            },
            
            {
                target: 'UC Davis',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'UC Davis',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'Chicago',
                value: 4
            },
            {
                target: 'UC San Diego',
                source: 'Cornell',
                value: 2
            },
            {
                target: 'UC San Diego',
                source: 'Duke',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'Emory',
                value: 3
            },
            {
                target: 'UC San Diego',
                source: 'Harvard',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'Tulane',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'UC Berkeley',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'UC San Diego',
                value: 2
            },
            {
                target: 'UC San Diego',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'UC San Diego',
                source: 'Washington',
                value: 1
            },
          
            
            {
                target: 'UCLA',
                source: 'Arizona',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Duke',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Florida',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Harvard',
                value: 3
            },
            {
                target: 'UCLA',
                source: 'Hawaii',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Michigan',
                value: 6
            },
            {
                target: 'UCLA',
                source: 'MIT',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'NYU',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Pennsylvania State',
                value: 2
            },

            {
                target: 'UCLA',
                source: 'Stanford',
                value: 4
            },
            {
                target: 'UCLA',
                source: 'UC Berkeley',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'UC San Diego',
                value: 2
            },
  
            
            {
                target: 'UCLA',
                source: 'UCLA',
                value: 2
            },
       
           
            {
                target: 'UCLA',
                source: 'UT Austin',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'Washington',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'Wisconsin Madison',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Yale',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Chicago',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Colorado',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Duke',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'UC San Diego',
                value: 1
            },
           
            {
                target: 'UCLA',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Rutgers',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Brown',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'JHU',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Michigan',
                value: 3
            },
            {
                target: 'UCLA',
                source: 'MIT',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'NYU',
                value: 2
            },
            {
                target: 'UCLA',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'UCLA',
                source: 'UC Berkeley',
                value: 5
            },
        
            {
                target: 'UCLA',
                source: 'Yale',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Colorado',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'CUNY',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Georgia',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'IUB',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'Northwestern',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'Pennsylvania',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'SUNY Stony Brook',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'JHU',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Michigan',
                value: 3
            },
            {
                target: 'UIUC',
                source: 'MIT',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'NYU',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'UC Berkeley',
                value: 1
            },
        
            {
                target: 'UIUC',
                source: 'UC Davis',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'UCLA',
                value: 2
            },
            {
                target: 'UIUC',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'UT Austin',
                value: 3
            },
            {
                target: 'UIUC',
                source: 'Washington',
                value: 1
            },
            {
                target: 'UIUC',
                source: 'Yale',
                value: 3
            },
            {
                target: 'UNC',
                source: 'Arizona',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Chicago',
                value: 2
            },
            {
                target: 'UNC',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Emory',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Florida',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'UNC',
                source: 'Michigan',
                value: 2
            },
            {
                target: 'UNC',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Pennsylvania',
                value: 2
            },
            {
                target: 'UNC',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Princeton',
                value: 1
            },
            {
                target: 'UNC',
                source: 'SUNY Albany',
                value: 1
            },
            {
                target: 'UNC',
                source: 'SUNY Binghamton',
                value: 1
            },

            {
                target: 'UNC',
                source: 'UCLA',
                value: 1
            },
            {
                target: 'UNC',
                source: 'UIUC',
                value: 1
            },
            {
                target: 'UNC',
                source: 'UNC',
                value: 3
            },
            {
                target: 'UNC',
                source: 'Yale',
                value: 1
            },
            {
                target: 'UNC',
                source: 'UT Austin',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Arizona State',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Colorado',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Cornell',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Duke',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Michigan',
                value: 1
            },
            {
                target: 'UNC',
                source: 'New Mexico',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Northwestern',
                value: 1
            },
            {
                target: 'UNC',
                source: 'UC San Diego',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Oregon',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Pennsylvania State',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Rutgers',
                value: 2
            },
            {
                target: 'UNC',
                source: 'Rutgers',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Brown',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Columbia',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Harvard',
                value: 2
            },
            {
                target: 'UNC',
                source: 'JHU',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Michigan',
                value: 3
            },
            {
                target: 'UNC',
                source: 'MIT',
                value: 1
            },
            {
                target: 'UNC',
                source: 'NYU',
                value: 2
            },
            {
                target: 'UNC',
                source: 'Pennsylvania',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Stanford',
                value: 1
            },
            {
                target: 'UNC',
                source: 'Syracuse',
                value: 1
            },
            {
                target: 'UNC',
                source: 'UC Berkeley',
                value: 5
            },
            {
                target: 'UNC',
                source: 'Yale',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Chicago',
                value: 6
            },
            {
              target: 'UT Austin',
                source: 'Columbia',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Columbia',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Cornell',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Duke',
                value: 2
            },
            {
              target: 'UT Austin',
                source: 'Florida',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Harvard',
                value: 2
            },
            {
              target: 'UT Austin',
                source: 'Hawaii',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'JHU',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Michigan',
                value: 3
            },
            {
              target: 'UT Austin',
                source: 'Ohio State',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Pennsylvania',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'Stanford',
                value: 2
            },
            {
              target: 'UT Austin',
                source: 'SUNY Stony Brook',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'UC Berkeley',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'UC Davis',
                value: 2
            },
            {
              target: 'UT Austin',
                source: 'UCLA',
                value: 1
            },
            {
              target: 'UT Austin',
                source: 'UT Austin',
                value: 3
            },
            {
              target: 'UT Austin',
                source: 'Vanderbilt',
                value: 1
            },
            {
              target: 'Utah',
                source: 'GWU',
                value: 1
            },
            {
              target: 'Utah',
                source: 'Michigan',
                value: 2
            },
            {
              target: 'Utah',
                source: 'New Mexico',
                value: 3
            },
            {
              target: 'Utah',
                source: 'Pennsylvania State',
                value: 1
            },
            {
              target: 'Utah',
                source: 'Stanford',
                value: 1
            },
            {
              target: 'Utah',
                source: 'UCLA',
                value: 1
            },
            {
              target: 'Utah',
                source: 'Washington',
                value: 1
            },
            {
              target: 'Utah',
                source: 'Yale',
                value: 1
            },
        ]
    }]
};
 </script>
   <script type="text/javascript">
    var myChart = echarts.init(document.getElementById('water'));
    window.onresize = function() {
    myChart.resize();
  };
  myChart.setOption(option);
  </script>
</body>
</html>
