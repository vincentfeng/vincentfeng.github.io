---
layout: post
title: JfreeCharts Demo
category: jfreecharts
tags: [java, jfreecharts]
---

Creating charts with JFreeChart is a three step process. You need to:
1. create a dataset containing the data to be displayed in the chart;
2. create a JFreeChart object that will be responsible for drawing the chart;
3. draw the chart to some output target (often, but not always, a panel on the screen);

使用JfreeChart 创建chart，请参加下面三个步骤：
1. 创建一个数据集的容器，用于呈现图形。
2. 创建一个JFreeChart 的对象，用于画图
3. 将信息放到图里面。

```java
package com.demo;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.ChartFrame;
import org.jfree.chart.JFreeChart;
import org.jfree.data.general.DefaultPieDataset;

public class Demo1 {
	public static void main(String[] args) {
		// step 1 
		DefaultPieDataset dataset = new DefaultPieDataset();
		dataset.setValue("Category 1", 43.2);
		dataset.setValue("Category 2", 27.9);
		dataset.setValue("Category 3", 79.5);
		
		// step 2 create a chart...
		JFreeChart chart = ChartFactory.createPieChart(
		"Sample Pie Chart",
		dataset,
		true, // legend?
		true, // tooltips?
		false // URLs?
		);
		
		// step 3 create and display a frame...
		ChartFrame frame = new ChartFrame("Test", chart);
		frame.pack();
		frame.setVisible(true);
	}
}
```