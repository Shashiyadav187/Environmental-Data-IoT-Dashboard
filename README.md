# Environmental-Data-IoT-Dashboard

This is a simple azure website that shows how to visualize data from eventhub as a real-time graph using D3.js.

## Acknowledgements

This code was modified from the sample [ThingLabs-IoT-Dashboard](https://github.com/ThingLabsIo/ThingLabs-IoT-Dashboard)




## Stream Analytics Job

```SQL
WITH ProcessedData as (
    SELECT
        MAX(Celsius) MaxTemperature,
        MIN(Celsius) MinTemperature,
        AVG(Celsius) AvgTemperature,
        Geo as location,
        DeviceId as deviceId,
        System.Timestamp AS Timestamp
    FROM
        [inputdata]
--    WHERE 
--        DeviceId = 'sparkfun'
    GROUP BY
        TumblingWindow (second, 15), DeviceId, Geo
)

-- Make sure this matches your Event Hub output name from above,
-- If you've forgotten it you can go back and get it in another browser tab
SELECT * INTO [outputdata] FROM ProcessedData
SELECT * INTO [outputblob] FROM ProcessedData
```

## Stream Analytics Event Queue Output

Note, the output type for Event hub from the Stream Analytics job must be 'Array'

![Stream Analytics Event Hub Output](https://raw.githubusercontent.com/gloveboxes/Environmental-Data-IoT-Dashboard/master/resources/StreamAnalyticsEventHubOutput.png)