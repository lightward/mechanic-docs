# Liquid profiling

If you're not sure why your task script might be slow, enable Liquid profiling. This feature times the execution of each Liquid statement, giving you insight into where your task is spending more or less time.

The Liquid profiling option can be found in Advanced mode for each task.

Once enabled, subsequent runs of the task will include a new section at the end of their results, labeled "Liquid performance profile":

![](https://s3.amazonaws.com/helpscout.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e1ae1f204286364bc93c3b6/5e1ae188264b8.png)

Each object in the profile includes three attributes: `"code"`, `"line_number"`, and `"render_time"` (measured in seconds). Liquid nodes that have children also have a `"children"` attribute, containing an array of timing objects for each child node.
