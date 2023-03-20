# dump		ftoken = requests.get("https://business.facebook.com/business_locations", headers=head, cookies = {"cookie":fbcokis}).text
		eaag = re.search("(EAAG\w+)",str(ftoken))
	except:
		slp(1)
		login()
	global totaldmp,count
	try:
		token=open('data/token.txt','r').read()
		fbcokis = open("data/cookie.txt", "r").read()
	except FileNotFoundError:
		print("[*] Login Not Found")
		slp(1)
		cmd('rm -rf data/token.txt')
		login()
	try:
		cmd('clear')
		os.system("clear")
		print(logo)
		now = datetime.datetime.now()
		print("Date and Time ")
		print(now.strftime("%y-%m-%d %H:%M:%S"))
		hostnm = socket.gethostname()
		ipaddress = socket.gethostbyname(hostnm)
		print("ip address is :", ipaddress)
		print("_____________________________________")
		try:
			print("cookies already login")
			fbbuid = input("[->] Enter Public ID Link : ")
			dmp = requests.get("https://graph.facebook.com/"+fbbuid+"?fields=friends.limit(5000)&access_token="+token,cookies = {"cookie":fbcokis}).json()
			for idnm in dmp['friends']['data']:
				totaldmp+=1
				fbidz.append(idnm['id'])
		except KeyError:
			print("[*] Public ID Not Found")
			main()
		filepath = input("[>>] Enter File Path : ")
		print("")
		print(47*"\033[1;37;1m-")
		apnd = open(filepath,'w')
		for fbuid in fbidz:
			count += 1
			try:
				dmp = requests.get("https://graph.facebook.com/"+fbuid+"?fields=friends.limit(5000)&access_token="+token,cookies = {"cookie":fbcokis}).json()
				for idnm in dmp['friends']['data']:
					apnd.write(idnm['id']+"|"+idnm['name']+'\n')
				print("\x1b[1;92m[>>] Dumping UID From : " + fbuid)
			except KeyError:
				print('\x1b[1;91m[>>] Dumping UID From : ' + fbuid)
		apnd.close()
		print(47*"\033[1;37;1m-")
		ch_x1 = input("[->] DoYou Want to Use DuplicateID Cuter (n/y) : ")
		if ch_x1 in ["yes","Yes","YES","Y","y"]:
			newfile = input("[->] File Without Duplicate ID Save As : ")
			os.system('sort -r '+filepath+' | uniq > '+newfile)
			ch_x2 = input("[->] DoYou Want to Use ID Separator (n/y) : ")
			if ch_x2 in ["yes","Yes","YES","Y","y"]:
				grep(newfile)
			else:
				print(47*"-")
				print (f"\x1b[0;37m Your Dump File Save As :\x1b[1;92m {newfile} \x1b[0;37m")
				print(47*'-')
				input("[>>] Press Inter to go Back < ")
				menu()
		else:
			print('\n')
			ch_x2 = input("[->] Do You Want to Use ID Separator (n/y) : \033[1;32;1m")
			if ch_x2 in ["yes","Yes","YES","Y","y"]:
				grep(filepath)
			else:
				print(47*'\033[1;37;1m-')
				print (f"\x1b[0;37m Total ID Dump :\x1b[1;92m {totaldmp}")
				print (f"\x1b[0;37m Your Dump File Save As :\x1b[1;92m {filepath} ")
				print(47*'\033[1;37;1m-')
				input("[>>] Press Inter to go Back < ")
				menu()
	except Exception as e:
		print("[>>] Error : %s"%e)
		exit("")
if __name__ == '__main__':
	Jadu()
