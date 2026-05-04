# using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        double[] data = { 115, 182, 191, 31, 196, 1099, 5, 172, 10, 179, 83, 21, 20, 21, 186, 177, 195, 193, 188, 199, 62, 109, 105, 183, 110 };
        Array.Sort(data);
        int n = data.Length;

        // 1. حساب الإحصائيات الأساسية
        double mean = data.Average();
        double median = GetPercentile(data, 50);
        double mode = data.GroupBy(x => x).OrderByDescending(g => g.Count()).First().Key;
        double variance = data.Select(x => Math.Pow(x - mean, 2)).Sum() / n;
        double stdDev = Math.Sqrt(variance);
        
        double p20 = GetPercentile(data, 20);
        double q1 = GetPercentile(data, 25);
        double q2 = median; // Q2 هو نفسه الوسيط
        double q3 = GetPercentile(data, 75);
        double iqr = q3 - q1;
        double range = data.Max() - data.Min();

        // طباعة النتائج
        Console.WriteLine($"Mean: {mean:F2}\nMedian: {median}\nMode: {mode}");
        Console.WriteLine($"Variance: {variance:F2}\nStd Deviation: {stdDev:F2}");
        Console.WriteLine($"P20: {p20}\nP50: {median}\nQ1: {q1}\nQ2: {q2}\nQ3: {q3}");
        Console.WriteLine($"Range: {range}\nIQR: {iqr}");

        // 2. تحديد القيم الشاذة (Outliers)
        double lowerBound = q1 - 1.5 * iqr;
        double upperBound = q3 + 1.5 * iqr;

        Console.WriteLine("\n--- Outliers Check ---");
        foreach (var x in data)
        {
            if (x < lowerBound || x > upperBound)
                Console.WriteLine($"{x} is an Outlier");
        }
    }

    // دالة لحساب المئينات (Percentiles)
    static double GetPercentile(double[] sortedData, double percentile)
    {
        double i = (percentile / 100.0) * (sortedData.Length - 1);
        int index = (int)Math.Floor(i);
        double fraction = i - index;
        if (index + 1 < sortedData.Length)
            return sortedData[index] + (fraction * (sortedData[index + 1] - sortedData[index]));
        return sortedData[index];
    }
}

