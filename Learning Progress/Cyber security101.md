# TryHackMe Cyber security 101 Course

# סיכום Active Directory Basics

## מה למדתי

- מבנה של AD הכולל בתוכו את בסיס הנתונים ו DC (Domain Controller)
- סוגי ישויות כגון משתמשים, מכונות, סוגי קבוצות ועוד
- רקע כללי על OUs (Organizational Units)
- יצירה ומחיקה של OUs על ידי הגדרות מתקדמות
- שימוש ב-Delegation
- Group Policy Objects (GPO)
- הגדרה של פעולות שונות על משתמשים כמו ביטול גישה לפאנל השליטה

## Windows Command Line

- מה זה ipconfig ומה מטרתו ושימוש בפקודה
- מה זה Ping ושימוש בפקודה
- פקודת tracert
- פקודת nslookup
- פקודת netstat
- פקודות cd וdir
- פקודות בסיסיות type, copy, move, erase/del
- שימוש במנהל המשימות עם פקודת tasklist
- פקודות בסיס בpowershell כמו: get-childitem, set-location, new-item, remove-item
- שימוש באופרטורים כמו -eq, -ge, -gt, -le

- תרגול מציאת דגל כחלק מהמודל מצורפים צילומי מסך
  ![](../screenshots/1.png)
  ![](../screenshots/2.png)
  ![](../screenshots/3.png)

- היכרות עם עוד פקודות כמו Get-Process, Get-Service ועוד הקשורות לנתוני מערכת מתקדמים

## Networking

- OSI Model 7 השכבות
- TCP/IP model
- Subnet
- UDP ו- TCP
- שימוש בפקודה telnet
- Dhcp
- Arp protocol
- ICMP
- Routing
- NAT
- DNS protocol
- שימוש בפקודת whois
- HTTPS protocol
- ftp protocol
- POP3 ו-IMAP
- SMTP/S protocol
- TLS protocol
- SSH protocol והשימוש בו
- הסבר על SFTP ו-FTPS
- VPN רקע כללי ושימוש

- תרגול פענוח תעבורת TLS מוצפנת באמצעות Wireshark
  ![](../screenshots/4.png)
  ![](../screenshots/5.png)

## WireShark basics

- מבנה התוכנה
- צביעת פקטות
- סינון פקטות בתצורות שונות
- מבנה של כל פקטה וממה היא בנויה
- ניווט בין פקטות
- ייצוא מידע/קובץ מתוך פקטות

## Tcpdump basics

- שימוש בפגקודת TCPDUMP
- שימוש בדגלים שונים בפקודה
- הוספת פילטרים שונים
- תצוגת פקטות על ידי הפקודה

## Nmap Basics

- שימוש בפקודה לצורך גילוי מכשירים זמינים ברשת (Live hosts)
- שימוש לסריקת פורטים
- שימוש בדגלים שונים בפקודה
- גילוי גרסאות ומערכות הפעלה של מכשירים ברשת
- שמירת סריקה לקובץ

## Cryptography Basics

- חשיבות הצפנה
- הבדל בין טקסט רגיל למוצפן
- סוגי הצפנות כמו סימטרית ואסימטרית
- רקע כללי על הצפנת RSA
- שימוש בSSH
- חתימות דיגיטליות ותעודות
- רקע כללי על pgp ו-gpg
- שימוש בgpg להצפנה ופענוח של מידע
- רקע כללי על מפתחות פרטיים וציבוריים סוגי הצפנות שונות ועוד
- רקע על RSA וSSH

## Hashing basics

- רקע על סוגי Hash שונים כמו SHA256 וMD5 ועוד
- הסבר על מה זה מילון ועל סוג ההתקפה הזאת (dictionary attack)
- שימוש בhashcat ותרגול לפענוח hashes

## John the ripper

- רקע כללי על כלי התוכנה john
- הבנה של איך הכלי עובד
- סינטקס בסיסי לפענוח Hashes שונים כגון MD5 SHA1 ועוד

- תרגול מעשי של פענוח Hashes
  ![](../screenshots/6.png)
  ![](../screenshots/7.png)
  ![](../screenshots/8.png)
  ![](../screenshots/9.png)

  - פענוח Hash מetc/shadow סיסמת root של המערכת
    ![](../screenshots/10.png)

- פענוח סיסמה של קובץ zip בעזרת כלי zip2john וחילוץ המידע הדרוש
  ![](../screenshots/11.png)
  ![](../screenshots/12.png)

- פענוח סיסמה של קובץ rar בעזרת כלי rar2john וחילוץ המידע הדרוש
  ![](../screenshots/13.png)
  ![](../screenshots/14.png)

  - פענוח סיסמת המפתח ssh על ידי כלי ssh2john
    ![](../screenshots/15.png)

  ## Moniker Link

  - רקע על מנגנון Moniker Link ב-Windows והשימוש בקובצי .url כ-Shortcut למשאבי רשת
  - הבנה כיצד פתיחת Moniker Link גורמת לשליחת אימות NTLM אוטומטי
  - היכרות עם NTLM hash והסיכון שבהדלפת credentials ללא הרצת קוד
  - תרגול תרחיש של Credential Harvesting באמצעות SMB/WebDAV

  ## MetaSploit

  - היכרות עם Metasploit Framework והמבנה הכללי שלו
  - הבנה של ההבדל בין exploit, payload ו-module
  - שימוש ב-msfconsole לאיתור והרצת exploits
  - עבודה עם payloads מסוג reverse shell ו-
  - ניצול שירות פגיע לצורך קבלת גישה ראשונית (Initial Access)
  - הבנת שלב ה-post exploitation והרחבת שליטה במערכת
  - תרגול סריקה ואימות חולשות לפני הרצת exploit

## Burp suite

- הבנה של מבנה התוכנה ושימוש בה
- שימוש ב Proxy, Repeater
- שימוש ב Foxy proxy
- שימוש ב target ליצירת מפה של האתר

## Hydra

- שימוש בפקודות לפריצת סיסמאות על ידי Brute force במסגרת המעבדה
- הבנת מבנה פקודה ודגלים של התוכנה

## תובנות אישיות

- איך בנוי AD
- סוגי ישויות שונות שנכללות בכל התחום הזה
- מה זה OUs ומה מטרתם
- איך להשתמש בהאצלת סמכויות בתוכנה (Delegation)
- מה זה GPO איך להגדיר אותו ואיך ליישם אותו על מחלקות שלמות/משתמשים
- איך לנהל באופן כללי חברה קטנה תחת AD הגדרת חוקים והחלתן על המכונות/משתמשים
- מבנה הOSI הבנה כיצד עובד האינטרנט באופן כללי
- עבודה מעשית עם תוכנת Wireshark והבנה שלה
- איך לסרוק רשתות ולגלות מידע חיוני
- חשיבות הצפנות והבנה של האלגוריתמים השונים הקיימים
- הבנה מה זה Hash מה החוזקות ומה החולשות שלו
- שימוש בכלי Hashcat
- שימוש בכלי john לפענוח ססמאות בעזרת wordlist
- שימוש בjohn לפענוח סיסמאות מערכת כגון לינוקס (פענוח סיסמת root)
- פענוח סיסמה לקבצי zip בעזרת zip2john
