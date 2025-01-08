import pandas as pd
import numpy as np

def get_signals(data, short_window=40, long_window=100):
    # محاسبه میانگین متحرک کوتاه و بلند مدت
    signals = pd.DataFrame(index=data.index)
    signals['price'] = data['Close']
    signals['short_mavg'] = data['Close'].rolling(window=short_window, min_periods=1, center=False).mean()
    signals['long_mavg'] = data['Close'].rolling(window=long_window, min_periods=1, center=False).mean()

    # ایجاد سیگنال خرید و فروش
    signals['signal'] = 0.0
    signals['signal'][short_window:] = np.where(signals['short_mavg'][short_window:] > signals['long_mavg'][short_window:], 1.0, 0.0)
    signals['positions'] = signals['signal'].diff()

    return signals

# بارگذاری داده‌های تاریخی قیمت
data = pd.read_csv('crypto_price_data.csv', index_col='Date', parse_dates=True)

# دریافت سیگنال‌ها
signals = get_signals(data)

# نمایش سیگنال‌ها
print(signals)



 
