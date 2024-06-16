from tkinter import *
import tkinter as tk
from tkinter import ttk
import yfinance as yf

# Global değişkenler
yeni_islem = True
hesap = []
s1 = []

def get_stock_price_yahoo(symbol):
    try:
        stock = yf.Ticker(symbol)
        hist = stock.history(period="1d", interval="1m")
        if not hist.empty:
            latest_price = hist['Close'].iloc[-1]
            return latest_price
        else:
            print("Geçersiz Sembol veya Veri Yok")
            return None
    except Exception as e:
        print(f"Hata: {e}")
        return None

# Yaz fonksiyonu
def yaz(x):
    global yeni_islem
    if yeni_islem:
        total_sonuc.delete(0, 'end')
        yeni_islem = False
    focused_widget = window.focus_get()
    if isinstance(focused_widget, ttk.Entry):
        s = len(focused_widget.get())
        focused_widget.insert(s, str(x))

# Sil fonksiyonu
def sil():
    focused_widget = window.focus_get()
    if isinstance(focused_widget, ttk.Entry):
        focused_widget.delete(len(focused_widget.get()) - 1)

# Temizle fonksiyonu
def temizle():
    global yeni_islem
    global hesap
    global s1
    
    # Pencere içindeki tüm ttk.Entry widget'larını bul ve temizle
    for widget in window.winfo_children():
        if isinstance(widget, ttk.Entry):
            widget.delete(0, 'end')
    
    yeni_islem = True
    hesap = []
    s1 = []

def hisse_goruntule():
    symbol = hisse_giris.get()
    try:
        lot_sayisi = int(Lot_giris.get())
    except ValueError:
        hisse_sonuc_giris.delete(0, 'end')
        hisse_sonuc_giris.insert(0, 'Geçersiz Lot Sayısı')
        return
    
    price = get_stock_price_yahoo(symbol)
    if price is not None:
        birim = secim_var1.get()
        
        # Her zaman 1 lot fiyatını gösterecek
        sonuc = f"{price:.2f} {birim} $"
        
        # Toplam fiyatı da göstermek isterseniz, bunu ayrıca hesaplayabilirsiniz
        if lot_sayisi > 1:
            total_price = price * lot_sayisi
            total_sonuc.delete(0, 'end')
            total_sonuc.insert(0, f"{total_price:.2f} {birim}")
        
        hisse_sonuc_giris.delete(0, 'end')
        hisse_sonuc_giris.insert(0, sonuc)
    else:
        hisse_sonuc_giris.delete(0, 'end')
        hisse_sonuc_giris.insert(0, 'Hata!')

def sadece_rakam_girisi(char):
    return char.isdigit()        

# Pencere ayarları
window = tk.Tk()
window.title('Porföy Hesaplama')
window.geometry("293x580")
window.configure(background='black')
window.resizable(width=False, height=False)

# Seçim baloncuğu için bir StringVar oluştur
secim_var1 = tk.StringVar()
secim_var2 = tk.StringVar()

# Hisse kodu girişi
hisse_giris_text = tk.StringVar()
hisse_giris = ttk.Entry(window, width=29, justify=tk.RIGHT, font=("Times",10), textvariable=hisse_giris_text)
hisse_giris.place(height=30, width=130, x=15, y=22)
Label = tk.Label(hisse_giris, text="Hisse Kodu", fg="black", bg="white", font=("Helvetica", 9))
Label.place(height=15, width=70, x=0, y=0)

# Girilen metni büyük harfe çevirme
hisse_giris_text.trace_add("write", lambda *args: hisse_giris_text.set(hisse_giris_text.get().upper()))

# Lot sayısı girişi
Lot_giris = ttk.Entry(window, width=29, justify=tk.RIGHT, font=("Times",10), validate="key")
Lot_giris['validatecommand'] = (Lot_giris.register(sadece_rakam_girisi), '%S')
Lot_giris.place(height=30, width=130, x=15, y=60)
Label = tk.Label(Lot_giris, text="Lot sayısı", fg="black", bg="white", font=("Helvetica", 9))
Label.place(height=15, width=60, x=0, y=0)

# Hisse senedi sonucu
hisse_sonuc_giris = ttk.Entry(window, width=29, justify=tk.RIGHT, font=("Times",10))
hisse_sonuc_giris.place(height=30, width=135, x=150, y=22)
Label = tk.Label(hisse_sonuc_giris, text="Fiyat", fg="black", bg="white", font=("Helvetica", 9))
Label.place(height=15, width=30, x=0, y=0)

toplam_fiyat = ttk.Entry(window, width=29, justify=tk.RIGHT, font=("Helvetica", 16))
toplam_fiyat.place(height=50, width=265, x=15, y=170)
Label = tk.Label(toplam_fiyat, text="Toplam Fiyat", fg="black", bg="white", font=("Helvetica", 9))
Label.place(height=15, width=80, x=0, y=0)


total_sonuc = ttk.Entry(window, width=29, justify=tk.RIGHT, font=("Helvetica", 16))
total_sonuc.place(height=50, width=265, x=15, y=100)
Label = tk.Label(total_sonuc, text="Toplam Fiyat", fg="black", bg="white", font=("Helvetica", 9))
Label.place(height=15, width=80, x=0, y=0)

# Seçenekler listesi
secenekler = ["$", "TRY", "EUR", "GBP", "JPY", "CNY", "RUB", "NZD", "CHF", "CAD", "HKD"]

combobox1 = ttk.Combobox(toplam_fiyat, textvariable=secim_var1, values=secenekler, state="readonly")
combobox1.place(width=60, height=17, x=0, y=35)

# Hisse senedi butonu
ttk.Button(window, text="Hisse Fiyat", command=hisse_goruntule, width=20).place(height=44, x=20, y=230)

# Silme ve temizleme butonları
style = ttk.Style()
style.configure('TButton', font=('Helvetica', 16))

# Sayı butonları
buttons = [
    {"text": "C", "command": temizle, "x": 105, "y": 280},
    {"text": "⌫", "command": sil, "x": 195, "y": 280},
    {"text": "1", "command": lambda: yaz(1), "x": 15, "y": 340},
    {"text": "2", "command": lambda: yaz(2), "x": 105, "y": 340},
    {"text": "3", "command": lambda: yaz(3), "x": 195, "y": 340},
    {"text": "4", "command": lambda: yaz(4), "x": 15, "y": 400},
    {"text": "5", "command": lambda: yaz(5), "x": 105, "y": 400},
    {"text": "6", "command": lambda: yaz(6), "x": 195, "y": 400},
    {"text": "7", "command": lambda: yaz(7), "x": 15, "y": 460},
    {"text": "8", "command": lambda: yaz(8), "x": 105, "y": 460},
    {"text": "9", "command": lambda: yaz(9), "x": 195, "y": 460},
    {"text": ".", "command": lambda: yaz("."), "x": 15, "y": 340},
    {"text": "0", "command": lambda: yaz(0), "x": 17, "y": 520, "width": 9}
]

# Butonları oluştur ve yerleştir

for button in buttons:
    ttk.Button(
        window, 
        text=button["text"], 
        command=button["command"], 
        width=button.get("width", 6)
    ).place(height=44, x=button["x"], y=button["y"])

window.bind("<Tab>", lambda event: "break")
hisse_sonuc_giris.bind("<FocusIn>", lambda event: "break")
hisse_sonuc_giris.bind("<Button-1>", lambda event: "break")

window.mainloop()
