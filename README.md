# Coursera-Exploratory-Data-Analysis-project1
This is the first project of the Coursera Exploratory Data Analysis first project 
Introduction

### Data overlook : 

This assignment uses data from the UC Irvine Machine Learning Repository, a popular repository for machine learning datasets. In particular, we will be using the "Individual household electric power consumption Data Set" which I have made available on the course web site:

Dataset: Electric power consumption [20Mb] # In the repository 

Description: Measurements of electric power consumption in one household with a one-minute sampling rate over a period of almost 4 years. Different electrical quantities and some sub-metering values are available.

The following descriptions of the 9 variables in the dataset are taken from the UCI web site:

Date: Date in format dd/mm/yyyy
Time: time in format hh:mm:ss
Global_active_power: household global minute-averaged active power (in kilowatt)
Global_reactive_power: household global minute-averaged reactive power (in kilowatt)
Voltage: minute-averaged voltage (in volt)
Global_intensity: household global minute-averaged current intensity (in ampere)
Sub_metering_1: energy sub-metering No. 1 (in watt-hour of active energy). It corresponds to the kitchen, containing mainly a dishwasher, an oven and a microwave (hot plates are not electric but gas powered).
Sub_metering_2: energy sub-metering No. 2 (in watt-hour of active energy). It corresponds to the laundry room, containing a washing-machine, a tumble-drier, a refrigerator and a light.
Sub_metering_3: energy sub-metering No. 3 (in watt-hour of active energy). It corresponds to an electric water-heater and an air-conditioner.
Loading the data

When loading the dataset into R, please consider the following:

The dataset has 2,075,259 rows and 9 columns. First calculate a rough estimate of how much memory the dataset will require in memory before reading into R. Make sure your computer has enough memory (most modern computers should be fine).

We will only be using data from the dates 2007-02-01 and 2007-02-02. One alternative is to read the data from just those dates rather than reading in the entire dataset and subsetting to those dates.

You may find it useful to convert the Date and Time variables to Date/Time classes in R using the strptime() and as.Date() functions.

Note that in this dataset missing values are coded as ?.


### LOAD THE  Electric power consumption DATASET 
```
install.packages("pryr")
library(pryr)

testMemory = read.table("./Downloads/household_power_consumption.txt", sep = ";", nrows = 1)
memory_estimation = object_size(testMemory)*2075259 # 9.55GB seems strange 

data = read.table("./Downloads/household_power_consumption.txt", sep = ";", header = T)
data$Date = as.Date(data$Date, )
data$Time = strptime(data$Date)
str(data_subseted)

data_subseted <- data[data$Date == '1/2/2007' | data$Date == '2/2/2007',]
data_subseted[,-c(1,2)] = sapply(data_subseted[,-c(1,2)], as.numeric) # Factors features to  numeric
data_subseted$Date <- as.Date(data_subseted$Date, format="%d/%m/%Y")
str(data_subseted)
```


### First plot : 
```
par(mfrow=c(1,1))
png("plot1.png", width=480, height=480)
hist(data_subseted$Global_active_power, main="Global Active Power", 
     xlab="Global Active Power (kilowatts)", ylab="Frequency", col="Red")
png("plot1.png", width=480, height=480)
dev.off()
```
https://github.com/rdpeng/ExData_Plotting1/blob/master/figure/unnamed-chunk-2.png

### Second plot :
```
### Transformaton of dates and times do weeks days 
library(dplyr)
data_subseted = 
  data_subseted%>%
  mutate(day = as.POSIXct(paste(Date, Time)))

png("plot2.png", width=480, height=480)

with(data_subseted,
  plot(Global_active_power~day, type="l",
       ylab="Global Active Power (kilowatts)", xlab=""))

dev.off()
```
https://github.com/rdpeng/ExData_Plotting1/blob/master/figure/unnamed-chunk-3.png

### Third plot : 
```
png("plot3.png", width=480, height=480)

plot(data_subseted$Sub_metering_1~data_subseted$day, type="l",
       ylab="Global Active Power (kilowatts)", xlab="Sorry it's in french")
lines(data_subseted$Sub_metering_2~data_subseted$day,col='Red')
lines(data_subseted$Sub_metering_3~data_subseted$day,col='Blue')

legend("topright", col=c("black", "red", "blue"), lty=1, lwd=2, 
       legend=c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"))
dev.off()
```
https://github.com/rdpeng/ExData_Plotting1/blob/master/figure/unnamed-chunk-4.png

### Fourth plot 
```
png("plot4.png", width=480, height=480)
?par
par(mfrow=c(2,2), mar=c(5,5,2,2), oma=c(0,0,2,0))
# mar = c(bottom, left, top, right)
with(data_subseted, {
  plot(Global_active_power~day, type="l", 
       ylab="Global Active Power (kilowatts)", xlab="")
  plot(Voltage~day, type="l", 
       ylab="Voltage (volt)", xlab="")
  plot(Sub_metering_1~day, type="l", 
       ylab="Global Active Power (kilowatts)", xlab="")
  lines(Sub_metering_2~day,col='Red')
  lines(Sub_metering_3~day,col='Blue')
  legend("topright", col=c("black", "red", "blue"), lty=1, lwd=2, bty="n",
         legend=c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"))
  plot(Global_reactive_power~day, type="l", 
       ylab="Global Rective Power (kilowatts)",xlab="")
})
dev.off()
```
https://github.com/rdpeng/ExData_Plotting1/blob/master/figure/unnamed-chunk-5.png
