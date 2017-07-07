# Error
> 100024 The encryption passphrase provided does not match with passphrase associated with the following server: %ServerName Please update the server settings with the current passphrase for data encryption.

# Summary
According to a handful of posts this error can be "resolved" by deleting the server from the Azure Backup UI. Although this is somewhat true, the documentation is old and lacks explanation of the root cause.  Surely, re-registering a server with a different encrytpion key will throw this error. The solution then should be the use of the correct encryption key and the root cause is probably bad key management. However, if key management is not the issue then the root cause is something different. In my case, the problem was two servers with the same name uploading into the same Resource Pool.

# Solution
## Incorrect approach.
The solution below assumes you are using an off-site backup with Files and Folders. Your backup may be slightly different.  

- Access Azure Portal
- Recovery Service Vaults
- Your vault
- Recovery Services Infrastructure
- Protected Servers 
- Azure Backup Servers
- Now delete the suspected server.

Ok, but now what? Register. Sure! However, what does that really get you.

## Correct approach.

If your cause was poor key management, you need to get better at managing keys. The solution above worked, but the issue represents a problem in your IT management.  Fix that, and then the issue is not a factor. You should not be in a situation where you are re-registering servers, and if you are, then it should be with the correct key.  

## Special Case - duplicate names.

Azure Backup's *Recovery Service Vaults seemingly do not support two servers with the same name*.  Seemingly, a second server coming in with the same name will attempt to claim the spot of an existing one, but of course clash on the Encryption Key.  The solution to this situation is the create additional Recovery Service Vaults and then re-register the servers with updated configurations.  This, unfortunately, means a timeconsuming re-do of the registration and also the re-selection of all of your folders/files.  
