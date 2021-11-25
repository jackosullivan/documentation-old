# Outlook

## Outlook automatically connecting to Office365 instead of Exchange

​When this occurs, this usually means that your Office365 mailbox has been added to Outlook rather than Exchange. This can happen even when you have selected Exchange Server during setup in some circumstances. 

You can confirm that this is the case within Outlook by going to File > Info at which you should see that the account settings indicate that Outlook is connecting to https://outlook.office365.com/owa/ which is incorrect. 

To fix this, we need to make a change to the Windows Registry. You will need to configure the following registry key:

```
HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\office\16.0\outlook\autodiscover
DWORD: ExcludeExplicitO365Endpoint
Value = 1
```

A reboot should then resolve the issue with Outlook connecting to Office365 instead of Exchange.
