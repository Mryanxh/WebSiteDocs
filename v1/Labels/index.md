﻿<h3 class="important-tittle">Labels</h3>

<p>
    A Label is any string representation of any value in the chart, they are normally placed over the axis length and in tool tips.
</p>

<div class="text-center">
    <img ng-src="{{source}}/v1/Labels/labels.jpg"/>
</div>

<div class="doc-alert">
    To keep this example always up to date it is directly pulled from the Github repository, the
    repo for simplicity uses the <i>Page</i> class to wrap every example, but you can use any
    container for your plots.
</div>

<h4>Code Behind</h4>

<pre class="prettyprint" ng-if="wpf" url="https://raw.githubusercontent.com/beto-rodriguez/Live-Charts/master/Examples/Wpf/CartesianChart/Labels/LabelsExample.xaml.cs"></pre>
<pre class="prettyprint" ng-if="uwp" url="https://raw.githubusercontent.com/beto-rodriguez/Live-Charts/master/Examples/Uwp/CartesianChart/Labels/LabelsExample.xaml.cs"></pre>
<pre class="prettyprint" ng-if="wf" url="https://raw.githubusercontent.com/beto-rodriguez/Live-Charts/master/Examples/WinForms/Cartesian/Labels/Labels.cs"></pre>

<h4 ng-if="uwp ||wpf">XAML</h4>

<pre class="prettyprint" ng-if="wpf" url="https://raw.githubusercontent.com/beto-rodriguez/Live-Charts/master/Examples/Wpf/CartesianChart/Labels/LabelsExample.xaml"></pre>
<pre class="prettyprint" ng-if="uwp" url="https://raw.githubusercontent.com/beto-rodriguez/Live-Charts/master/Examples/Uwp/CartesianChart/Labels/LabelsExample.xaml"></pre>

<h4>1. Axis Labels</h4>

<p>
    An <i>Axis</i> has 2 types of labels, formatted and mapped labels.
</p>

<h4>1.1 Formatted Labels <small class="text-muted">As seen at Y axis</small></h4>

<p>
    A Formatted label is useful when there is a direct conversion between the chart value and the label, 
    for example, in the image above, the Y axis has a range of values between 8 and 26, but because of 
    the current formatter, we are able to see '8' as '8.00k items'.
</p>

<p>
    To achieve this use the <i>Axis.LabelFormatter</i> property, it stores a function that takes a double 
    value as parameter and returns a string, LiveCharts will use this function every time it needs to display 
    a chart value as <i>string</i>.
</p>

<pre class="prettyprint">MyAxis.LabelFormatter = val => val.ToString("C"); //as currency
MyAxis.LabelFormatter = val => val + "°"; //as degrees
MyAxis.LabelFormatter = val => val + ".00 items sold"; //or any other custom format</pre>

<h4>1.2 Mapped Labels <small class="text-muted">As seen at X axis</small></h4>

<p>
    A mapped label is normally useful to map a position with a name, for example when first 
    point belongs to John, the second to Susan and the third one to Charles.
</p>

<pre class="prettyprint" ng-if="wpf">&lt;lvc:BarChart&gt;
  &lt;lvc:BarChart.AxisX&gt;
    &lt;lvc:Axis Title="Month" Labels="John, Susan, Charles" /&gt;
  &lt;/lvc:BarChart.AxisX&gt;
&lt;/lvc:BarChart&gt;</pre>

<pre class="prettyprint" ng-if="uwp">Labels = new string[] {"John", "Charles", "Susan"};</pre>

<pre class="prettyprint" ng-if="uwp">&lt;lvc:BarChart&gt;
  &lt;lvc:BarChart.AxisX&gt;
    &lt;lvc:Axis Title="Month" Labels="{Binding Labels}" /&gt;
  &lt;/lvc:BarChart.AxisX&gt;
&lt;/lvc:BarChart&gt;</pre>

<pre class="prettyprint" ng-if="wf">cartesianChart1.AxisX.Add(new LiveCharts.Wpf.Axis
{
   Labels = new string[] {"John", "Charles", "Susan"}
});</pre>

<p>
    Labels are mapped according to the position (zero-indexed) of each label in our labels collection,
    when a chart requires to draw a label for <i>X = 0</i>, it will pull <i>Labels[0]</i>, when <i>X = 1</i>
    the label will be <i>Labels[1]</i> .... when <i>X = n</i> => <i>Labels[n]</i>.
</p>

<p>
    Notice that if the axis value is greater than <i>Labels.Count</i> an empty string will be returned.
</p>

<p>
    <i>Axis.Labels</i> hides <i>Axis.LabelFormatter</i>, therefore if <i>Axis.Labels</i> is not null 
    then labels will be pulled from <i>Axis.Labels</i>, if <i>Axis.Labels</i> is null then LiveCharts 
    will use the formatter, if both are null then the raw value will be used as label.
</p>

<h4>2. Data Labels</h4>

<p>
    Any time you need a label on every point of your series (<small class="text-muted">as shown in image</small>), 
    set the <i>Series.DataLabels</i> property to <i>true</i>.
</p>

<p>
    If necessary, you can also customize the DataLabel format using the <i>Series.LabelPoint</i> property
</p>

<pre class="prettyprint">new ColumnSeries
{
   Values = new ChartValues&lt;double> { 4, 2, 8, 2, 3, 0, 1 },
   DataLabels = true,
   LabelPoint = point => point.Y + "K"
}</pre>

<h4>Rotating labels</h4>

<p>
    Some times your labels are long, and you need to optimize the space, in this case you can rotate any axis labels.
    You can use any angle, even negatives.
</p>

<pre class="prettyprint" ng-if="wpf || uwp">&lt;lvc:CartesianChart Grid.Row="2" Series="{Binding SeriesCollection}"&gt;
  &lt;lvc:CartesianChart.AxisX&gt;
    &lt;lvc:Axis LabelsRotation="13" Labels="{Binding Labels}"&gt;
      &lt;lvc:Axis.Separator&gt;
        &lt;!--
        force the separator step to 1, so it always display all labels
        if you don't force the separator, it will be calculated 
        automatically, and could skip some labels,
        disable it to make it invisible
        -->
        &lt;lvc:Separator IsEnabled="False" Step="1"&gt;
          &lt;/lvc:Separator&gt;
        &lt;/lvc:Axis.Separator&gt;
      &lt;/lvc:Axis&gt;
    &lt;/lvc:CartesianChart.AxisX&gt;
  &lt;lvc:CartesianChart.AxisY&gt;
&lt;/lvc:CartesianChart&gt;</pre>

<pre class="prettyprint" ng-ig="wf">cartesianChart1.AxisX.Add(new LiveCharts.Wpf.Axis
{
   Labels = new[]
   {
      "Shea Ferriera",
      "Maurita Powel",
      "Scottie Brogdon"
   },
   LabelsRotation = 13,
   Separator = new Separator // force the separator step to 1, so it always display all labels
   {
     Step = 1, // if you don't force the separator, it will be calculated automatically, and could skip some labels
     IsEnabled = false //disable it to make it invisible.
   }
});</pre>