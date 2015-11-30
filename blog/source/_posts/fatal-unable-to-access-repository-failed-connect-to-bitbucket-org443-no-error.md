title: "Fatal: unable to access 'Repository': Failed connect to bitbucket.org:443; No error"
tags:
  - BitBucket
  - Cisco VPN
  - Git
id: 5631
categories:
  - Code Dive
date: 2015-03-12 23:05:00
---

I was getting this error when trying to push a new branch up to bitbucket today:
<pre class="csharpcode">fatal: unable to access <span class="str">'https://{account}@bitbucket.org/{project}/{repo-name}.git/'</span>: Failed connect to bitbucket.org:443; No error</pre>
<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>

I was able to get the correct IP for bitbucket.org via nslookup, and I was able to access both HTTP and HTTPS in a browser. Going verbose didn’t reveal much new information either:
<pre class="csharpcode">$ GIT_CURL_VERBOSE=1 git push --set-upstream origin feature/select-account-dialog:feature/select-account-dialog
  * Couldn't find host bitbucket.org in the _netrc file; using defaults
  * Adding handle: conn: 0x36b570
  * Adding handle: send: 0
  * Adding handle: recv: 0
  * Curl_addHandleToPipeline: length: 1
  * - Conn 0 (0x36b570) send_pipe: 1, recv_pipe: 0
  * About to connect() to bitbucket.org port 443 (#0)
  *   Trying 131.103.20.168...
  * Timed out
  *   Trying 131.103.20.167...
  * Timed out
  * Failed connect to bitbucket.org:443; No error
  * Closing connection 0
fatal: unable to access 'https://{account}@bitbucket.org/{project}/{repo-name}.git/': Failed connect to bitbucket.org:443; No error</pre>
<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>

Then I noticed the Cisco VPN client icon in the taskbar…I was connected to a VPN. Disconnected, and the push was successful. Hope this helps someone else out there.

Haping coding. ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/03/wlEmoticon-smile.png)
