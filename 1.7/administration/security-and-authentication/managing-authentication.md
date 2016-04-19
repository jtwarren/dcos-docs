---
post_title: Managing Authentication
menu_order: 1
---
Authentication is managed in the DC/OS web interface.

You can authorize individual users. You can grant access to users who are local or remote to your datacenter. 

The DC/OS user database is persisted in ZooKeeper running on the master nodes in znodes under the path `/dcos/users`.

Tokens sent to DC/OS in a HTTP Authorization header must be of the format `token=<token>` instead of the `Bearer <token>`. Both formats
will be supported in a future release.

# User management

Users are granted access to DC/OS by another authorized user. A default user is automatically created by the first user that logs in to the DC/OS cluster.

To manage users:

1.  Launch the DC/OS web interface and login with your username (Google, GitHub, and Microsoft) and password.

2.  Click on the **System** -> **Organization** tab and choose your action.
    
    ### Add users
    
    From the **Users** tab, click **New User** and fill in the new user email address. New users are automatically sent an email notifying them of access to DC/OS.
    
    ![new DC/OS user](../img/ui-add-user.gif)
 
    ### Delete users
    
    1.  From the **Users** tab, select the user name and click **Delete**. 
    2.  Click **Delete** to confirm the action.
    
    ### Switch users
    
    To switch users, you must log out of the current user and then back in as the new user.
    
    *   To log out of the DC/OS web interface, click on your username in the lower left corner and select **Sign Out**.
        
        ![log out](../img/auth-enable-logout-user.gif)
        
        You can now log in as another user.
    
    *   To log out of the DC/OS CLI, enter the this command:
        
            $ dcos config unset core.dcos_acs_token
            Removed [core.dcos_acs_token]
            
        
        You can now log in as another user.


# Logging in to the DC/OS CLI
Authentication is only supported for DC/OS CLI version 0.4.3 and above. See [here](/docs/1.7/usage/cli/update/) for upgrade instructions.

The DC/OS CLI stores the token in a configuration file in the `.dcos` directory under the home directory of the user running the CLI. This token can be used with the curl command to access DC/OS APIs, using curl or wget. For example, `curl -H 'Authorization: token=<token>' http://cluster`.

1.  To login to the CLI, run this command and follow the instructions:

        $ dcos auth login
        
        Please go to the following link in your browser:
        
            https://<public-master-ip>/login?redirect_uri=urn:ietf:wg:oauth:2.0:oob
        
        Enter authentication token: 
        
1.  Log in through your browser.
    
    ![alt](../img/auth-login.gif)
    
1.  Copy the link and paste in your terminal.
    
    ![alt](../img/auth-login-token.gif)
    
    You should see output similar to:
    
        Enter authentication token: eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQwHY5KAUm2ttXB01yqMLal5sVCgYhbLkZtFKRYxfNV-AK1IPbw7zlxXUQ
        [core.dcos_acs_token]: set
        Login successful!
    
    You are now authorized!
    
1.  To logout, run this command:

        dcos auth logout
        
## Debugging

To debug authentication problems, refer to the Admin Router and dcos-oauth logs on the masters, by running `sudo journalctl -u dcos-adminrouter.service`
or `sudo journalctl -u dcos-oauth.service`, respectively.

## Authentication opt-out

If you are performing a custom advanced installation, you may opt out of
Auth0-based authentication by adding the following directive to
`genconf/config.yaml` (note that the quotes are currently required):

```yaml
oauth_enabled: 'false'
```

On AWS, when creating a stack, at the Specify Details step, you may choose to
set the `OAuthEnabled` option to `false` to disable authentication for the DC/OS
installation.

The same option is not currently available when installing DC/OS through the
Azure Marketplace, but will be added in a future release along with other
options to customize authentication.

## Ad Blockers and the DC/OS UI

During testing, we have observed issues with loading the DC/OS UI login page
when certain ad blockers such as HTTP Switchboard or Privacy Badger are active.
Other ad blockers like uBlock Origin are known to work.

## Further reading

- [Let’s encrypt DCOS!](https://mesosphere.com/blog/2016/04/06/lets-encrypt-dcos/):
  a blog post about using [Let's Encrypt](https://letsencrypt.org/) with
  services running on DC/OS.

## Future work

We are looking forward to working with the DC/OS community on improving existing
security features as well as on introducing new ones in the coming releases.

 [1]: https://en.wikipedia.org/wiki/STARTTLS