
```r
# generate plots
plist = list()
x = 1
for (i in unique(data$geneName)) {
  p = ggplot(data[data$geneName %in% i, ], aes(x = group1, y = value, fill = group1)) +
        geom_violin(scale = "count", position = position_dodge(width = 1), trim = F) +
        geom_boxplot(aes(x = group1, y = value, color = group2), position = position_dodge(width = 0.5), notch = F) +
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
