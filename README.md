# encoding: utf-8
import requests as r, re, os
from bs4 import BeautifulSoup as par
import platform

if platform.python_version().split(".")[0] != "3":
	exit("use: python lock.py")

#-> Color
m, p, b, h = "\x1b[1;91m", "\x1b[1;97m", "\x1b[1;94m", "\x1b[1;92m"
#---------

ua = {"user-agent":"chrome"}
prvt = []
ses = r.Session()
link = "https://free.facebook.com/"

def login(kukis):
	cok = {"cookie":kukis}
	cek = par(ses.get(link+"/home.php?ref=dbl", cookies=cok).text, "html.parser")
	if "mbasic_logout_button" in str(cek):
		print("%s[>]%s Login berhasil" % (b,h))
		if "Apa yang Anda pikirkan sekarang" in str(cek):
			pass
		else:
			try:
				xnxx = par(ses.get(link+"/language.php", cookies=cok).text, "html.parser").find("a",string="Bahasa Indonesia")["href"]
				ses.get(link+xnxx, cookies=cok)
			except:
				pass
		print("%s[>]%s Tunggu Sebentar Sedang Memproses" % (b,p))
		return cok["cookie"]
	else:
		exit("%s[×]%s Cookie Invalid..\n" % (m,p))

def locked(kukis):
	cok = {"cookie":kukis}
	cek = par(ses.get(link+"/home.php?ref=dbl", cookies=cok).text, "html.parser")
	for x in cek.find_all("a",string=" + "):
		ges = par(ses.get(link+x.get("href"), cookies=cok).text, "html.parser")
		e = ges.find("a",string="Bahasa Burma")["href"]
		gse = par(ses.get(link+e, cookies=cok).text, "html.parser")
		acc = gse.find("a",{"accesskey":"7"}).get("href")
		set = par(ses.get(link+acc, cookies=cok).text, "html.parser")
		gex = re.findall(r"/private_sharing/home_view/\?entry_point=settings\&amp;profile_id=\d+\&amp;refid=\d+", str(set))
		prvt.append("".join(gex))
	act = par(ses.get(link+"".join(prvt), cookies=cok).text, "html.parser")
	ac = act.find("form").get("action")
	dt = act.find("input",{"name":"fb_dtsg"}).get("value")
	jz = act.find("input",{"name":"jazoest"}).get("value")
	data = {"fb_dtsg":dt,"jazoest":jz}
	pos = ses.post(link+ac, data=data, cookies=cok).text
	try:
		xnxx = par(ses.get(link+"/language.php", cookies=cok).text, "html.parser").find("a",string="Bahasa Indonesia")["href"]
		ses.get(link+xnxx, cookies=cok)
	except:
		pass
	print("%s[✓]%s Profile Locked Sudah Aktif\n" % (h,p))

def main():
	logo = """
%s.____                  %s__              .___
%s|    |    ____   ____ %s|  | __ ____   __| _/
%s|    |   /  _ \_/ ___\%s|  |/ // __ \ / __ |
%s|    |__(  <_> )  \___%s|    <\  ___// /_/ |
%s|_______ \____/ \___  %s>__|_ \\___  >____ |
%s        \/          \/  %s   \/    \/     \/
    ----------------------------------
%s      Author%s:%s Mexsi
%s      Team%s  :%s BCC
%s      Github%s:%s github.com/Mexsi-Id04
%s    ----------------------------------
""" % (b,p,b,p,b,p,b,p,b,p,b,p,b,m,h,b,m,h,b,m,h,p)
	print(logo)
	print("%s[>]%s Login FB Dengan Cookies Anda" % (b,p) )
	kue = input("%s[+] %sCookies%s:%s " % (b,p,m,h))
	extensi = login(kue)
	locked(extensi)


if __name__=="__main__":
	os.system("git pull")
	os.system("clear")
	main()#FF1D4E
