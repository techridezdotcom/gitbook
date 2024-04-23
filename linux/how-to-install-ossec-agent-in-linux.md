---
description: >-
  OSSEC is a multiplatform, open source and free Host Intrusion Detection System
  (HIDS), for this tutorial I am using oracle linux
cover: ../.gitbook/assets/ossec (1).jpg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# How to Install Ossec agent in linux

{% hint style="info" %}
please run following command.&#x20;
{% endhint %}

```bash
wget -q -O - https://updates.atomicorp.com/installers/atomic | sudo bash
```

{% hint style="info" %}
this will be the terminal output
{% endhint %}

{% code title="terminal output" lineNumbers="true" %}
```bash
Atomic Free Unsupported Archive installer, version 7.3.0

BY INSTALLING THIS SOFTWARE AND BY USING ANY AND ALL SOFTWARE
PROVIDED BY ATOMICORP LIMITED YOU ACKNOWLEDGE AND AGREE:

THIS SOFTWARE AND ALL SOFTWARE PROVIDED IN THIS REPOSITORY IS
PROVIDED BY ATOMICORP LIMITED AS IS, IS UNSUPPORTED AND ANY
EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL ATOMICORP LIMITED, THE
COPYRIGHT OWNER OR ANY CONTRIBUTOR TO ANY AND ALL SOFTWARE PROVIDED
BY OR PUBLISHED IN THIS REPOSITORY BE LIABLE FOR ANY DIRECT,
INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED
OF THE POSSIBILITY OF SUCH DAMAGE.

For supported software packages please contact us at:

  sales@atomicorp.com

Do you agree to these terms? (yes/no) [Default: yes]

Configuring the [atomic] repo archive for this system

Installing the Atomic GPG keys: OK

Downloading atomic-release-1.0-23.el9.art.noarch.rpm: Verifying...                          ################################# [100%]
Preparing...                          ################################# [100%]
Updating / installing...
   1:atomic-release-1.0-23.el9.art    ################################# [100%]

Enable repo by default? (yes/no) [Default: yes]:


The Atomic repo has now been installed and configured for your system
The following channels are available:
  atomic          - [ACTIVATED] - contains the stable tree of ART packages
  atomic-testing  - [DISABLED]  - contains the testing tree of ART packages
  atomic-bleeding - [DISABLED]  - contains the development tree of ART packages

```
{% endcode %}

{% hint style="warning" %}
install ossec agent in redhat based system, if you are using ubuntu then use apt instead of yum
{% endhint %}

```bash
sudo yum install ossec-hids-agent
```

{% hint style="info" %}
please find the terminal output
{% endhint %}

{% code title="terminal output" lineNumbers="true" %}
```bash
Rocky / Red Hat Enterprise Linux 9 - atomic                                         113 kB/s | 182 kB     00:01
Dependencies resolved.
====================================================================================================================
 Package                        Architecture      Version                            Repository                Size
====================================================================================================================
Installing:
 ossec-hids-agent               x86_64            1:3.7.0-27751.el9.art              atomic                   212 k
Installing dependencies:
 GeoIP                          x86_64            1.6.12-7.el9                       atomic                   115 k
 GeoIP-GeoLite-data             noarch            1.6.12-5.el9                       atomic                   651 k
 expect                         x86_64            5.45.4-15.el9                      ol9_appstream            275 k
 mariadb-connector-c            x86_64            3.2.6-1.el9_0                      ol9_appstream            205 k
 ossec-hids                     x86_64            1:3.7.0-27751.el9.art              atomic                    51 k

Transaction Summary
====================================================================================================================
Install  6 Packages

Total download size: 1.5 M
Installed size: 4.3 M
Is this ok [y/N]: y
Downloading Packages:
(1/6): ossec-hids-3.7.0-27751.el9.art.x86_64.rpm                                     77 kB/s |  51 kB     00:00
(2/6): GeoIP-1.6.12-7.el9.x86_64.rpm                                                150 kB/s | 115 kB     00:00
(3/6): expect-5.45.4-15.el9.x86_64.rpm                                              2.0 MB/s | 275 kB     00:00
(4/6): mariadb-connector-c-3.2.6-1.el9_0.x86_64.rpm                                 6.3 MB/s | 205 kB     00:00
(5/6): GeoIP-GeoLite-data-1.6.12-5.el9.noarch.rpm                                   681 kB/s | 651 kB     00:00
(6/6): ossec-hids-agent-3.7.0-27751.el9.art.x86_64.rpm                              336 kB/s | 212 kB     00:00
--------------------------------------------------------------------------------------------------------------------
Total                                                                               866 kB/s | 1.5 MB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                            1/1
  Installing       : mariadb-connector-c-3.2.6-1.el9_0.x86_64                                                   1/6
  Installing       : expect-5.45.4-15.el9.x86_64                                                                2/6
  Installing       : GeoIP-GeoLite-data-1.6.12-5.el9.noarch                                                     3/6
  Installing       : GeoIP-1.6.12-7.el9.x86_64                                                                  4/6
  Running scriptlet: ossec-hids-1:3.7.0-27751.el9.art.x86_64                                                    5/6
  Installing       : ossec-hids-1:3.7.0-27751.el9.art.x86_64                                                    5/6
  Running scriptlet: ossec-hids-1:3.7.0-27751.el9.art.x86_64                                                    5/6
  Installing       : ossec-hids-agent-1:3.7.0-27751.el9.art.x86_64                                              6/6
  Running scriptlet: ossec-hids-agent-1:3.7.0-27751.el9.art.x86_64                                              6/6
  Running scriptlet: GeoIP-GeoLite-data-1.6.12-5.el9.noarch                                                     6/6
  Running scriptlet: ossec-hids-agent-1:3.7.0-27751.el9.art.x86_64                                              6/6
  Verifying        : GeoIP-1.6.12-7.el9.x86_64                                                                  1/6
  Verifying        : GeoIP-GeoLite-data-1.6.12-5.el9.noarch                                                     2/6
  Verifying        : ossec-hids-1:3.7.0-27751.el9.art.x86_64                                                    3/6
  Verifying        : ossec-hids-agent-1:3.7.0-27751.el9.art.x86_64                                              4/6
  Verifying        : expect-5.45.4-15.el9.x86_64                                                                5/6
  Verifying        : mariadb-connector-c-3.2.6-1.el9_0.x86_64                                                   6/6

Installed:
  GeoIP-1.6.12-7.el9.x86_64                             GeoIP-GeoLite-data-1.6.12-5.el9.noarch
  expect-5.45.4-15.el9.x86_64                           mariadb-connector-c-3.2.6-1.el9_0.x86_64
  ossec-hids-1:3.7.0-27751.el9.art.x86_64               ossec-hids-agent-1:3.7.0-27751.el9.art.x86_64

Complete!

```
{% endcode %}
