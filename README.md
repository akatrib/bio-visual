## Bio Visuals
Collection of major bio-themed data visuals &nbsp; `by Amal Katrib`
<br>

### Violin Plot
A box plot and kernel density plot hybrid that shows summary statistics as well as the full distribution of the data.
<p align="left">
  <img src="img/violin-plot.png" width = "40%" height = "50%"/>
</p>


Below is a sample code that can be used to generate violin plots in __R__:

```r
# load the appropriate packages
library(ggplot2)

# generate plots
plist = list()
x = 1
for (i in unique(data$geneName)) {
  p = ggplot(data[data$geneName %in% i, ], aes(x = group1, y = value)) +
        geom_violin(scale = "count", position = position_dodge(width = 1), trim = F) +
        geom_boxplot(aes(x = group1, y = value), notch = F) +
        geom_point(position = position_jitterdodge(jitter.width = 0.5), aes(color = group2)) +
        geom_vline(xintercept = c(x,y)) +
        labs(x = "", y = ""))

  plot_list[[x]] = p
  x = x + 1  }
names(plist) = unique(data$geneName)

# save plots
lapply(1:length(plot_list), function(i) {
           png("violionPlot.png"), 5, 5, res = 300, units = "in")
           print(plot_list[[i]])
           dev.off() })
```
&nbsp;
---

### Heatmap
A 2D visualization of hierarchical clustering with a color scale-rendition of numerical data to help reveal underlying patterns
<p align="left">
  <img src="img/heatmap.png" width = "70%"/>
</p>

I recommend using the following __[heatmap.3()](https://github.com/obigriffith/biostar-tutorials/blob/master/Heatmaps/heatmap.3.R)__ function if you want to include multiple row and column side bars for additional sample and gene info

Data inputs consist of:<br>
__[ data ]__ &nbsp; a matrix with log- variance stabilization-transformed normalized read counts (if you're in the next-gen sequencing space)<br>
__[ clab ]__ &nbsp; a matrix with color mapping of sample of info you are interested in showing

They will have the following structure:

|        | sample 1 | sample 2 | sample 3 | sample 4 |
|:-------|:--------:|:--------:|:--------:|:--------:|
| gene 1 |     3    |    10    |     9    |     5    |
| gene 2 |     9    |     4    |     6    |    10    |
| gene 3 |     3    |     6    |     6    |     9    |
| gene 4 |     8    |     6    |     8    |    10    |

|          | infoColor 1 | infoColor 2 | infoColor 3 | infoColor 4 |
|:---------|:-----------:|:-----------:|:-----------:|:-----------:|
| sample 1 |     red     |    yellow   |    orange   |   darkblue  |
| sample 2 |     red     |    green    |    black    |   darkred   |
| sample 3 |     blue    |    yellow   |    orange   |   darkblue  |
| sample 4 |     blue    |    yellow   |    black    |   darkblue  |


Below is a sample code that can be used to generate heatmaps in __R__:

``` r
hr <- hclust(as.dist(1-cor(t(data), method="pearson")), method="average")
hc <- hclust(as.dist(1-cor(data, method="pearson")), method="average")

heatmap.3(data, Rowv = as.dendrogram(hr), Colv = as.dendrogram(hc), dendrogram = "both", col = palette,
          ColSideColors = clab, key = TRUE)
```

Depending on what you intend to visualize, data can be scaled to mean = 0 & standard deviation = 1 either by:<br>
Setting the `scale` parameter in the heatmap function using  `heatmap.3(scale = "row" )`<br>
Directly scaling the matrix content using `t(scale(t(data))) `

&nbsp;
---
&nbsp;
