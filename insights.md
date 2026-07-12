Oct & Nov are always the highest (£1M+)	Clear holiday season spike — this company sells gifts/home décor
December drops sharply	Orders for Christmas are placed in Oct/Nov. By Dec, shopping is winding down
Jan–Mar are consistently lowest	Post-holiday slump — very common in retail
Data runs Dec 2009 → Dec 2011	Two full years — enough to confirm the seasonal pattern repeats

This is likely a B2B (business-to-business) company — the order sizes are too large for individual consumers. Customer 16446 spending £84,000+ per order screams wholesale. This context matters when framing your portfolio narrative.


R — Recency	How many days since their last purchase?	Are they a recent or lapsed customer?
F — Frequency	How many orders have they placed total?	Are they a one-time buyer or a loyal repeat customer?
M — Monetary	How much total money have they spent?	Are they a high-value or low-value customer?

The RFM scores are the input to KMeans clustering — instead of us manually deciding who belongs in which group, we let the algorithm find natural groupings in the data based on these three numbers.

	Customer ID	Recency	Frequency	Monetary	Segment
0	12346.0	326	17	-64.68	Lost
Customer 12346.0 has a negative Monetary value (-64.68). This means they had net returns exceeding purchases. Edge case — not worth fixing for this project, but worth noting if asked in an interview

The relationship between features (recency, frequency, monetary) and churn is fairly linear — which is exactly what Logistic Regression is built for
This is a key interview talking point: "I tested both models and let the data decide — simpler won"