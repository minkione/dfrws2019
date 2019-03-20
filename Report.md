# DFRWS Challenge 2018 - NIWC LANT

This repository is Naval Information Warfare Center, Atlantic's submission to the 2018 DFRWS Challenge.  More details about the challenge can be found on the [DFRWS Challenge GitHub page](https://github.com/dfrws/dfrws2018-challenge).

Submission by: Naval Information Warfare Center, Atlantic (NIWC LANT)

Team: Kelly Hines, Joshua Lewis, Nicholas Phillpott, Randy Sharo

Submission date: 20 MAR 2019

# Report

## Table of Contents
* [Executive Summary](#executive-summary)
* [Objectives](#objectives)
* [Evidence Review](#evidence)
* [Timeline](#timeline)
* [Conclusion](#conclusion)
* [Alternative Conclusions](#alternative-conclusions)
* [Acknowledgements](#acknowledgements)

## Executive Summary
From October 2018 through the submission deadline the Navy Information Warfare Center (NIWC LANT) performed a digital forensics examination of the devices listed in the Evidence section below. This report will outline the data extraction, alternative conclusions, and the tools developed to answer the questions presented by the challenge.

## Objectives
The attorney general assigned the following objectives:
* Determine the time the illegal drug lab was raided
* Determine which, if any, of Jessie Pinkman's friends were involved in the raid and document the confidence level in that hypothesis
* Determine how the QBee camera was disabled

## Evidence Review
Evidence items are listed in the order they were presented by the challenge documentation. Each analysis link shows the detailed extraction and analysis of each evidence item. Notably, to share this challenge among teams, evidence items are not original evidence but copies of the previously collected evidence.

| File Description | Filename | Analysis | SHA256 Hash |
| --- | --- | --- | --- |
| Jessie Pinkman’s Samsung phone | Samsung GSM_SM-G925F Galaxy S6 Edge.7z | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/Samsung%20GSM_SM-G925F%20Galaxy%20S6%20Edge.7z.md) | ae83b8ec1d4338f6c4e0a312e73d7b410904fab504f7510723362efe6186b757 |
| iSmartAlarm – Diagnostic logs | ismartalarm/diagnostics/2018-05-17T10_54_28/server_stream | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/ismartalarm/server_stream.md) | 8033ba6d37ad7f8ba22587ae560c04dba703962ed16ede8c36a55c9553913736 |
| iSmartAlarm – Memory images: 0x0000’0000 (ismart_00.img), 0x8000’0000 | dump/ismart_00.img, dump/ismart_80.img | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/ismart_00.img.md) | b175f98ddb8c79e5a1e7db84eeaa691991939065ae17bad84cdbd915f65d9a10 b175f98ddb8c79e5a1e7db84eeaa691991939065ae17bad84cdbd915f65d9a10  |
| Arlo – Memory image | arlo/dfrws_arlo.img |  | 3b957a90a57e5e4485aa78d79c9a04270a2ae93f503165c2a0204de918d7ac70 |
| Arlo – NVRAM settings | arlo/nvram.log |  | f5d680d354a261576dc8601047899b5173dbbad374a868a20b97fbd963dca798 |
| Arlo – NAND: TAR archive of the folder /tmp/media/nand | arlo/arlo_nand.tar.gz | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/arlo/nand.md) | 857455859086cd6face6115e72cb1c63d2befe11db92beec52d1f70618c5e421 |
| WinkHub – Filesystem TAR archive | wink/wink.tar.gz | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/wink_analysis.md) | 083e7428dc1d0ca335bbcfc11c6263720ab8145ffc637954a7733afc7b23e8c6 |
| Amazon Echo – Extraction of cloud data obtained via CIFT | echo/(2018-07-01_13.17.01)_CIFT_RESULT.zip | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/echo_analysis.md) | 7ee2d77a3297bb7ea4030444be6e0e150a272b3302d4f68453e8cfa11ef3241f |
| Network capture | network/dfrws_police.pcap | [Link](https://gitlab.com/lewis.joshua/dfrws2019/blob/master/network_analysis.md) | 1837ee390e060079fab1e17cafff88a1837610ef951153ddcb7cd85ad478228e |

### Lab Layout Diagram

![image](https://raw.githubusercontent.com/dfrws/dfrws2018-challenge/master/DFRWS2018-IoT-ForensicChallengeDiagram.png)

### User identities and associations

#### ISmartAlarm ids

| User | Most Likely Identity | Related accounts/identities | Notes |
| - | - | - | - |
| JPinkman | Jessie Pinkman | jpinkman2018@gmail.com | |
| TheBoss | S. Varga | | _associated with Varga by process of elimination_ |
| pandadodu | D. Pandana | | _associated with Pandana due to name similarity_ |
| | | emidnight@gmail.com | _email addr present in com.quirky.android.wink.wink_preferences.xml_ |
| | | francesco | _user id present in data/com.android.chrome/app_chrome/Default/Web Data_ |

#### fluffy
   * `fluffy@hogwarts` ssh key on Wink device.  Logged in as root from `172.21.94.4`
   * Multiple `Fluffy` sessions captured in `data/com.android.chrome/app_chrome/Default/Sync Data/SyncData.sqlite3`
   * `data/com.android.chrome/app_chrome/Default/Sync Data/LevelDB/000003.log` associates `Fluffy` with `Cthulhuuuu's iPhone` (Chrome IOS-PHONE)



## Steps to Reproduce

Due to the time that we had to invest in this challenge we chose to focus our efforts on two areas that were mentioned by the challenge creators.  Those are:
1.	Device Level Analysis: Developing methods and tools to forensically process digital traces generated by IoT devices, including on mobile devices.
4.	Evaluating and Expressing Conclusions: Assigning the probability of the results given two competing propositions (e.g. The prime suspect committed the offense, versus some unknown person did).

We created plaso parsers to support certain artifacts of interest and have provided them in our public fork of plaso. While we work to get the change merged, the source code is available for review and installation [here](https://gitlab.com/lewis.joshua/plaso).

## Timeline
| Date | Time | Event | Device Source | Notes |
| --- | --- | --- | --- | --- |
| 2018-05-17 | 10:22:20 | 30 seconds from this time date Alexa triggered iSmartAlarm to set the alarm even though the door was open | Alexa | Odd owner would set alarm with door open unless owner was wanting to set to home mode |
| 2018-05-17 | 10:40:00 | Police alerted an illegal drug lab was invaded and unsuccessfully set on fire | Challenge Details | |
| 2018-05-17 | 10:45:00 | Police and forensic team arrive on scene | Challenge Details | |

## Conclusion

## Alternative Conclusions

## Acknowledgements

The authors would like to thank the Digital Forensic Research Workshop and the developers of this years challenge for putting this activity together. Opportunities like this are critical to advancing the technolgies and methodoligies used by digital forensics examiners and researchers. Thank you. 