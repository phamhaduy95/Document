### Introduction

When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:

|**Term**|**Meaning**|**Think of it as...**|
|---|---|---|
|**Period**|The **length of time (in seconds)** CloudWatch aggregates metric data before evaluating.|The size of one data “window”|
|**Evaluation Periods**|The **number of most recent periods** that CloudWatch looks at to decide the alarm state.|The number of “windows” used for evaluation|
|**Datapoints to Alarm**|How many of those evaluation periods **must breach** the threshold before the alarm triggers.|The number of “bad windows” required to sound the alarm|

####  Alarm States

| **State**             | **Description**                                             |
| --------------------- | ----------------------------------------------------------- |
| **OK**                | The metric is within the defined threshold.                 |
| **ALARM**             | The metric breached the threshold (above or below).         |
| **INSUFFICIENT_DATA** | The alarm just started, no recent data, or data gaps exist. |

#### Creating new Alarms

``` bash
aws cloudwatch put-metric-alarm \
  --alarm-name "HighCPUAlarm" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 60 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
  --evaluation-periods 5 \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:NotifyOps \
  --treat-missing-data notBreaching

```

important parameter

| **Parameter**           | **Description**                                                                 |
| ----------------------- | ------------------------------------------------------------------------------- |
| `--metric-name`         | The metric to monitor (e.g., `CPUUtilization`).                                 |
| `--namespace`           | The metric namespace (e.g., `AWS/EC2`).                                         |
| `--statistic`           | Metric aggregation type (Average, Sum, Minimum, Maximum).                       |
| `--threshold`           | Value to compare against.                                                       |
| `--comparison-operator` | E.g., `GreaterThanThreshold`, `LessThanOrEqualToThreshold`.                     |
| `--evaluation-periods`  | How many consecutive data points must breach to trigger the alarm.              |
| `--period`              | How often data points are evaluated (e.g., 60 seconds).                         |
| `--treat-missing-data`  | How to handle missing points: `missing`, `notBreaching`, `breaching`, `ignore`. |

Option `--treat-missing-data`

When CloudWatch doesn’t receive new metric data, the alarm enters the `INSUFFICIENT_DATA` state — unless you specify a custom behavior.

This property defines what the alarm should do if a data point is missing.

|**Value**|**Meaning**|**Effect**|
|---|---|---|
|`missing` _(default)_|Missing data means “don’t evaluate”|Alarm enters `INSUFFICIENT_DATA`|
|`notBreaching`|Treat missing data as within normal range|Keeps alarm in `OK`|
|`breaching`|Treat missing data as out of range|Forces alarm into `ALARM`|
|`ignore`|Ignores missing data completely|Alarm continues as if all good|

#### Composite Alarms

A **Composite Alarm** combines multiple alarms using logical operators:
- **AND**, **OR**, **NOT**

Example:
> “Trigger alarm if either `HighCPUAlarm` OR `HighMemoryAlarm` is in ALARM state.”

Composite alarms reduce **noise** and let you track multiple related metrics together.