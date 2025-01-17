# Instance API

### setStyleOptions(options)
Set the style configuration.
- `options` style configuration, please refer to [style details](styles.md)



### getStyleOptions()
Get the style configuration.


### setPriceVolumePrecision(pricePrecision, volumePrecision)
Set the price and quantity precision.
- `pricePrecision` price accuracy, which affects the numerical accuracy of the price displayed on the entire chart, and also includes the technical indicator whose indicator series is price
- `volumePrecision` quantity accuracy, which affects the numerical accuracy of the quantity displayed on the entire chart, and also includes the technical index of the volume where the indicator series is


### setTimezone(timezone)
Set the time zone.
- `timezone` Time zone name, such as'Asia/Shanghai', if it is not set, the local time zone will be automatically obtained. Please search for the relevant documents for the name list of the time zone


### getTimezone()
Get the chart time zone.


### setZoomEnabled(enabled)
Set whether to zoom.
- `enabled` true or false


### isZoomEnabled()
Can zoom.


### setScrollEnabled(enabled)
Set whether to drag and scroll.
- `enabled` true or false


### isScrollEnabled()
Whether you can drag and scroll.


### setOffsetRightSpace(space)
Set the gap that can be left on the right side of the chart.
- `space` Distance size, number type


### setLeftMinVisibleBarCount(barCount)
Set the minimum number of candles visible on the left.
- `barCount` quantity, number type


### setRightMinVisibleBarCount(barCount)
Set the minimum number of candles visible on the right.
- `barCount` number, number type


### setDataSpace(space)
Set the space occupied by a piece of chart data, that is, the width of a single candlestick.
- `space` width, number type


### applyNewData(dataList, more)
Add new data, this method will clear the chart data, no need to call the clearData method.
- `dataList` is a KLineData array. For details of KLineData type, please refer to the data source
- `more` tells the chart if there are more historical data, it can be defaulted, the default is true


### applyMoreData(dataList, more)
Add more historical data.
- `dataList` is a KLineData array, please refer to the data source for details of KLineData type
- `more` tells the chart if there is more historical data, it can be the default, the default is true



### updateData(data)
Update data. Currently, only the current time stamp of the last data will be matched. If it is the same, it will be overwritten, and if it is different, it will be added.
- `data` Single bar data


### getDataList()
Get the current data source of the chart.


### clearData()
Clear chart data. Normally, clear data is to add new data. In order to avoid repeated drawing, all here is just to clear data, and the chart will not be redrawn.


### loadMore(cb)
Set to load more callback functions.
- `cb` is a callback method, the callback parameter is the timestamp of the first data


### createTechnicalIndicator(name, isStack, paneOptions)
Create a technical indicator, the return value is a string that identifies the window, which is very important, and some subsequent operations on the window require this identification.
- `name` Technical indicator name
- `isStack` is covered
- `paneOptions` window configuration information, which can be defaulted, `{ id, height, dragEnabled }`
  - `id` window id, default. Special paneId: candle_pane, the window id of the main image
  - `height` The height of the window
  - `dragEnbaled` window can be adjusted by dragging

Example:
```javascript
chart.createTechnicalIndicator('MA', false, {
  id:'pane_1',
  height: 100,
  dragEnabled: true
})
```


### overrideTechnicalIndicator(override)
Cover technical indicator information.
- `override` some parameters that need to be overridden, `{ name, calcParams, precision, styles }`
  - `name` technical indicator name, required field
  - `calcParams` calculation parameters, default
  - `precision` precision, default
  - `styles` style, which can be defaulted, and the `technicalIndicator` in the same style configuration is consistent

Example:
```javascript
chart.overrideTechnicalIndicator({
  name:'MA',
  calcParams: [5, 10, 30, 60, 120],
  precision: 4,
  styles: {
    bar: {
      upColor:'#26A69A',
      downColor:'#EF5350',
      noChangeColor:'#888888'
    },
    line: {
      size: 1,
      colors: ['#FF9600','#9D65C9','#2196F3','#E11D74','#01C5C4']
    },
    circle: {
      upColor:'#26A69A',
      downColor:'#EF5350',
      noChangeColor:'#888888'
    }
  }
})
```


### getTechnicalIndicatorByName(name)
Obtain technical indicator information according to the technical indicator name.
- `name` technical indicator name, can be the default, the default returns all


### getTechnicalIndicatorByPaneId(paneId)
Obtain technical indicator information according to the window id.
- `paneId` window id, which can be defaulted, and all are returned by default.

Special paneId: candle_pane, the window id of the main image


### removeTechnicalIndicator(paneId, name)
Remove technical indicators.
- `paneId` window id, which is the window ID returned when the createTechnicalIndicator method is called
Special paneId: candle_pane, the window id of the main image
- `name` technical indicator type, if default, all will be removed


### addCustomTechnicalIndicator(technicalIndicator)
Add a custom technical indicator.
- `technicalIndicator` technical indicator information, please refer to [Technical Indicators](./n50w7#mSzvS)


### createGraphicMark(name, options)
Create a graphic mark and return a string type identifier
- `name` Graphic mark type
- `options` configuration, `{ id, points, styles, onDrawStart, onDrawing, onDrawEnd, onClick, onRightClick, onPressedMove, onRemove }`
- `id` can be defaulted, if specified, the id will be returned
- `points` point information, can be defaulted, if specified, a graph will be drawn according to the point information
- `styles` style, can be defaulted, the format is the same as `graphicMark` in the configuration
- `onDrawStart` draw start callback event, can be default
- `onDrawing` callback event during drawing, can be default
- `onDrawEnd` draw end callback event, can be default
- `onClick` click callback event, default
- `onRightClick` right-click callback event, it can be defaulted, it needs to return a boolean type value, if it returns true, the built-in right-click delete will be invalid
- `onPressedMove` press and drag callback event
- `onRemove` delete callback event

Example:
```javascript
chart.createGraphicMark(
  'segment',
  {
    points: [
      {timestamp: 1614171282000, price: 18987 },
      {timestamp: 1614171202000, price: 16098 },
    ],
    styles: {
      line: {
        color:'#f00',
        size: 2
      }
    },
    onDrawStart: function ({ id }) {console.log(id) },
    onDrawing: function ({ id, step, points }) {console.log(id, step, points) },
    onDrawEnd: function ({ id }) {console.log(id) },
    onClick: function ({ id, event }) {console.log(id, event) },
    onRightClick: function ({ id, event }) {
      console.log(id, event)
      return false
    },
    onPressedMove: function ({ id, event }) {console.log(id, event) },
    onRemove: function ({ id }) {console.log(id)}
  }
)
```



### setGraphicMarkOptions(id, options)
Set the drawn graphic mark configuration.
- `id` calls the createGraphicMark method to return the identity
- `options` configuration, `{ styles }`
- `styles` style, the format is the same in the configuration of `graphicMark`


### addCustomGraphicMark(graphicMark)
Add custom graphic markers.
- `graphicMark` graphic mark information, please refer to [details](graphic-mark.md)


### removeGraphicMark(id)
Remove all graphic marks.
- `id` call the createGraphicMark method is the returned mark, if the default is, all marks will be removed


### subscribeAction(type, callback)
Subscribe to chart actions.
- `type` The type is 'drawCandle', 'drawTechnicalIndicator', 'zoom' and 'scroll'
- `callback` is a callback method


### unsubscribeAction(type, callback)
Unsubscribe from chart actions.
- `type` type is 'drawCandle', 'drawTechnicalIndicator', 'zoom' and 'scroll'
- `callback` callback method when subscribing


### getConvertPictureUrl(includeTooltip, includeGraphicMark, type, backgroundColor)
Get the picture url after the chart is converted into a picture.
- `includeTooltip` Whether to prompt the floating layer, it can be defaulted
- `includeGraphicMark` Whether to include the graphic mark layer, it can be defaulted
- `type` The converted picture type. The type is one of three types: 'png', 'jpeg', and 'bmp', which can be defaulted, and the default is 'jpeg'
- `backgroundColor` background color, can be the default, the default is '#333333'


### resize()
Resizing the chart will always fill the container size.
Note: This method will recalculate the size of each module of the entire chart. Frequent calls may affect performance. Please be cautious when calling
