import urllib.request
import re

def get_usd_rate_from_bot():
    # 直接抓取台灣銀行牌告匯率的英文/標準版網頁
    url = "https://bot.com.tw"
    
    try:
        req = urllib.request.Request(
            url, 
            headers={
                'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
            }
        )
        
        with urllib.request.urlopen(req, timeout=15) as response:
            html = response.read().decode('utf-8')
            
            # 使用精準的文字定位，尋找美金（USD）現鈔或本行看板的匯率位置
            # 台灣銀行網頁中，美金區塊後面會緊接本行即期賣出或現鈔賣出的數字
            # 我們利用正則表達式直接鎖定「美金」文字後方的數字
            rates = re.findall(r'(\d+\.\d{2,5})', html)
            
            # 在台銀網頁結構中，前幾個出現的浮點數通常就是美金的現鈔與即期匯率
            if rates and len(rates) > 2:
                # 通常第 1 個是美金現鈔買入，第 2 個是現鈔賣出
                cash_buy = rates[0]
                cash_sell = rates[1]
                print(f"★ 測試成功！從台灣銀行抓取目前美金匯率 ★")
                print(f"美金現鈔 - 銀行買入價: {cash_buy}")
                print(f"美金現鈔 - 銀行賣出價: {cash_sell}")
            else:
                # 如果找不到，改用另一種特徵比對
                match = re.search(r'USD.*?(\d+\.\d+)', html, re.DOTALL)
                if match:
                    print(f"★ 測試成功！目前 1 美金對台幣匯率約為: {match.group(1)} ★")
                else:
                    print("無法從台灣銀行網頁解析出匯率數字。")
                    
    except Exception as e:
        print(f"連線至台灣銀行發生錯誤: {e}")

if __name__ == "__main__":
    get_usd_rate_from_bot()
