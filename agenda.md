# Trust DID Web Work Item Rolling Agenda<!-- omit in toc -->

Zoom Link: [https://us02web.zoom.us/j/83119969275?pwd=IZTuXgGLtdLPjPLuB6q8zHXazxHSsU.1](https://us02web.zoom.us/j/83119969275?pwd=IZTuXgGLtdLPjPLuB6q8zHXazxHSsU.1)

Agenda: [HackMD](https://hackmd.io/k4cIK9vQSlaeg2pdHE51IQ), [TrustDIDWeb Repository](https://github.com/decentralized-identity/trustdidweb/blob/main/agenda.md) (synchronized after each meeting)

[**WG projects**](https://github.com/decentralized-identity?q=wg-cc&type=&language=) | [DIF page](https://identity.foundation/working-groups/claims-credentials.html) | [Mailing list and Wiki](https://lists.identity.foundation/g/cc-wg) | [Meeting recordings](https://docs.google.com/spreadsheets/d/1wgccmMvIImx30qVE9GhRKWWv3vmL2ZyUauuKx3IfRmA/edit?gid=111226877#gid=111226877)

## Table of Contents<!-- omit in toc -->

- [Meeting Information](#meeting-information)
- [Future Topics](#future-topics)
- [Meeting - 21 Nov 2024](#meeting---21-nov-2024)
- [Meeting - 07 Nov 2024](#meeting---07-nov-2024)
- [Meeting - 24 Oct 2024](#meeting---24-oct-2024)
- [Meeting - 10 Oct 2024](#meeting---10-oct-2024)
- [Meeting - 26 Sept 2024](#meeting---26-sept-2024)
- [Meeting - 12 Sept 2024](#meeting---12-sept-2024)

## Meeting Information

- Before you contribute - **[join DIF]** and [sign the WG charter] (both are required!)
- Meeting Time: Every second Thursday at 9:00 Pacific / 18:00 Central Europe
- [Calendar entry]
- [ID WG participation tracking]
- [Zoom room]
- Links and Repositories:
    - [Specification], [Spec Repository](https://github.com/decentralized-identity/trustdidweb), [Information Site](https://didtdw.org/)
    - Implementations: [TS](https://github.com/decentralized-identity/trustdidweb-ts), [Python](https://github.com/bcgov/trustdidweb-py), [Go](https://pkg.go.dev/github.com/nuts-foundation/trustdidweb-go)
    - [Trust DID Web Server](https://github.com/decentralized-identity/trustdidweb-server-py)

_Participants are encouraged to turn your video on. This is a good way to build rapport across the contributor community._

_This document is live-edited DURING each call, and stable/authoritative copies live on our github repo under `/agenda.md`, link: [Agenda]._

[join DIF]: https://identity.foundation/join
[sign the WG charter]: https://bit.ly/DIF-WG-select1
[Calendar entry]: https://calendar.google.com/event?action=TEMPLATE&tmeid=NG5jYWowbmZsdWNzM21tYjBsbDIzdG50ZzFfMjAyNDA5MTJUMTYwMDAwWiBkZWNlbnRyYWxpemVkLmlkZW50aXR5QG0&tmsrc=decentralized.identity%40gmail.com&scp=ALL
[Zoom Room]: https://us02web.zoom.us/j/83119969275?pwd=IZTuXgGLtdLPjPLuB6q8zHXazxHSsU.1
[DIF Code of Conduct]: https://github.com/decentralized-identity/org/blob/master/code-of-conduct.md
[ID WG participation tracking]: https://docs.google.com/spreadsheets/d/12hFa574v5PRrKfzIKMgDTjxuU6lvtBhrmLspfKkN4oE/edit#gid=0
[operations@identity.foundation]: mailto:operations@identity.foundation
[did:tdw Specification license]: https://github.com/decentralized-identity/trustdidweb/blob/main/LICENSE.md
[Agenda]: https://github.com/decentralized-identity/trustdidweb/blob/main/agenda.md
[Specification]: https://identity.foundation/trustdidweb

## Future Topics

- Using the `did:tdw` log format with other DID Methods
- Merging `did:tdw` features into `did:web`?
- Implementor's experiences -- architectures, learnings
- A did:tdw test suite -- such as proposed [here](https://github.com/nuts-foundation/trustdidweb-go/pull/1)

============================================
## Meeting - 21 Nov 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording and Chat Transcript](https://us02web.zoom.us/rec/share/UsNpfqags9z3oprz-DbXQZQX-xPEnJlBZhTn8AWfY-35hNdFkBk9QJQfVKXmU867.mbd57z9owlBoP-oi)

Attendees:

- Stephen Curran
- Sylvain Martel
- John Jordan
- Patrick St. Louis
- Andrew Whitehead
- Brian Richter
- Emiliano Sune
- Martina Kolpondinos
- Michael Herman
- Michel Sahli
- Rob Aaron
- Dmitri Zagidulin

1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
    5. Announcements:
        1. Add here...
2. DID Method Name Change:
    1. What's the new DID Method name going to be?  Leaning is towards `did:webl`. Let's discuss.
        1. Too easy to confuse with `did:web1`.
        2. Or perhaps we should use it instead of `did:webl`.
    3. An idea -- `did:web:<something>` but that doesn't sound aligned with the DID spec and would break the name space and `did:web` spec. Nope.
    4. `did:weblog` -- but that conflicts with `blog`.
    5. **`did:webh` -- "h" for history, emphasizing the log (history) capability, which is the important part. Loses the "secure", but so be it.**
    6. `did:webhash` as an alternative. Although it is bit subtle a connection to the purpose of the DID Method.
3. Version 0.5 Updates:
    1. Pre-rotation change -- [PR #129](https://github.com/decentralized-identity/trustdidweb/pull/129).  Summary:
        1. No pre-rotation:
            1. Each entry is signed by a key from the active `updateKeys` list.
            2. The `active` list is the most recently defined list prior to the current log entry -- except for the first entry, where it is the list in the first entry.
        2. With pre-rotation:
            1. Eliminate the `prerotation` parameter. Preroation is automatically activated when the `nextKeys` item is present in a DID log entry and **MUST** be enforced from then on.
            2. Can be activated in the first log entry, or afterwards. If activated afterwards, the current log entry is signed according to the "non-pre-rotation" rules, and the pre-rotation rules apply to all subsequent log entries.
            3. Once active, every entry **MUST** have a new `updateKeys` and `nextKeys` list.
            4. The hash of each `updateKeys` list item **MUST** be in the `nextKeys` list from the previous entry. This rule does not apply to the first log entry, as there is no previous entry.
            5. Each entry must be signed (in the DI proof) by a key in the `updateKeys` list from the current record.
    2.  Witness change requests from @andrewwhitehead [HackMD Doc](https://hackmd.io/TlvkAGw6RMq0i9gPanU1jg). Summary:
        1. Data model for the `witnesses` parameter remains the same (list of witnesses, threshold).
        2. Instead of the witness proofs going into the log, they are stored in a separate file, `witness.json` -- an array of proofs from witness, including the DID Log entry version number (1, 2, ...) to which the proof applies. *Or should it be the `version_id`?*
        3. The array contains only the last two proofs from each witness from which proofs are collected. When a witness proof is retrieved, if there are already two in the file from the witness, remove the oldest, and add the new one before writing the file.
        4. The processing is: Given an entry to be witnessed, verify that the `witness.json` file contains valid witness proofs from a threshold of current witnesses on the current or **newer** log entries.
        5. Questions from the document:
            1. Witness proofs can be published before the DID log with a new log entry is published. This eliminates the race condition of the DID Log being published without corresponding witness proofs. The "two proofs" per witness allows for the verification of at least one of the proofs of the witness when the `witness.json` is published before the corresponding log entry. The resolver would ignore the proof of an unpublished entry.
            2. What DID Method can a witness use? Currently must be `did:webh`, but could/should we allow `did:key` be used?  Could use "instantly resolved DIDs" -- e.g. `did:key`. Short list: `did:webh`, `did:key`, ~~`did:jwk`~~, maybe `did:web`.
                1. Long discussion resulted -- to be fully resolved in Discord and at the next meeting.
                2. It was proposed that there are two use cases for witnesses, one where there was just the added security of needing more than one key to sign the log entry (e.g. must compromize multiple keys and the web server to impact the DID), and another where the reputation of the witness was important in an ecosystem (external parties monitoring the behaviour of the DID Controller), so there needs to be ways to attach reputation (presumably, via VCs or a Trust Registry) to the witness.
                3. For the first - purely for added security - an ephemeral key method is sufficient -- `did:key`. We discussed also allowing `did:jwk` but it was felt that was just an alternative to `did:key` that adds complexity/dependencies to the spec without adding any value.
                4. For the second, the reputation could be added by a Trust Registry while using `did:key`, but it would eliminate (complicate?) the ability to use VCs for the reputation.
                5. For the second, some thought support for `did:webh` makes sense, while others are concerned about possiblity of resolution taking a long time if the witnesses have witnesses have witnesses, and so on. Worse is the scenario of DIDs having common witnesses, and the need to detect and handle an infinite loop.
                6. This then changed to a discussion, that also needs to be continued on Discord and at the next meeting about if a key from a prior log entry should be used to verify a signature in the `did:webh` scenario, and more broadly in the use of verifiable credentials signed using a key in a `did:webh` DIDDoc.
            2. NOT DISCUSSED: Should the addition of witnesses only be permitted in the first entry?
            3. NOT DISCUSSED: Is there a use case for turning off witnessing?
4. NOT DISCUSSED: *No progress made on the spec.* Latest spec updates and implementation notes.
    1. Cleaning up `[[spec]]` references -- Brian has enabled us to add our own spec references.
    2. Next up -- DRYing the. spec.
    3. Security and Privacy sections. Anyone able to help?
    4. Getting "spec to a standard" advice and applying those changes.
5. NOT DISCUSSED: DIDDoc and DID Metadata
6. NOT DISCUSSED: [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb/issues)
7. NOT DISCUSSED: Update on the [did:tdw Web Server](https://github.com/decentralized-identity/trustdidweb-server-py) -- Patrick St. Louis.
8. NOT DISCUSSED: Update: AnonCreds object formats and did:tdw, and perhaps a follow up discussion on [DID Linked Resources](https://w3c-ccg.github.io/DID-Linked-Resources/)
    1. Good discussion about the pros and cons of signing the resource, if it is signed should we use the VCDM or just attach a Data Integrity proof, and how we can get a consistent hash and where should it go.
    2. Alignment with the DID Linked Resources spec would really nice to have.  We don't want to return in the DIDDoc metadata the information about all the resources associated with a DID -- it can be a lot of data. That said, a discovery mechanism for resources, such as `<did>?resources` would be really nice.
    3. The next step is to create a document about a likely approach ([@andrewwhitehead](https://github.com/andrewwhitehead) agreed to create that first draft) and then we can then collaborate on implementing/updating the document from there.

## Meeting - 07 Nov 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording](https://us02web.zoom.us/rec/share/P4e70wPlORd-22ZfQz2nwCc4LiSphAgWEl7MuHMZxy9pIrE30EZXVh1Qc1-sXARe.1O6EbE-wmSAsgP67)

Attendees:

- Stephen Curran
- Patrick St. Louis
- Andrew Whitehead
- Brian Richter
- Cole Davis
- Emiliano Sune
- Kjetil Hustveit
- Martina Kolpondinos
- Michael Herman
- Michel Sahli
- Rob Aaron

1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
        1. Pre-rotation change to be put into a PR for discussion -- Michel Sahli
    6. Announcements:
        1. DID Methods Working Group Meetings start **next Wednesday, Nov. 13** at 9:00 Pacific / 18:00 Central Europe -- [calendar](https://calendar.app.google/h5sMifqRBHdt9Qjq7).
2. IIW Update
    1. did:tdw session
    2. `<did>/whois` as a separate specification and work item.
    3. Getting `<did>/whois` examples into the wild -- resolver support, did:tdw Server support.
3. *No progress made on this.* Latest spec updates and implementation notes.
    1. Cleaning up `[[spec]]` references -- Brian has enabled us to add our own spec references.
    2. Next up -- DRYing the. spec.
    3. Security and Privacy sections. Anyone able to help?
    4. Getting "spec to a standard" advice and applying those changes.
4. Registering did:tdw as a DID Method [PR](https://github.com/w3c/did-extensions/pull/581), and **[DONE!]** adding a did:tdw component to the [Universal Resolver](https://dev.uniresolver.io/) (thanks Brian!).
5. [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb/issues)
6. Update on the [did:tdw Web Server](https://github.com/decentralized-identity/trustdidweb-server-py) -- Patrick St. Louis.
7. AnonCreds object formats and did:tdw, and perhaps a follow up discussion on [DID Linked Resources](https://w3c-ccg.github.io/DID-Linked-Resources/)
    1. Good discussion about the pros and cons of signing the resource, if it is signed should we use the VCDM or just attach a Data Integrity proof, and how we can get a consistent hash and where should it go.
    2. Alignment with the DID Linked Resources spec would really nice to have.  We don't want to return in the DIDDoc metadata the information about all the resources associated with a DID -- it can be a lot of data. That said, a discovery mechanism for resources, such as `<did>?resources` would be really nice.
    3. The next step is to create a document about a likely approach ([@andrewwhitehead](https://github.com/andrewwhitehead) agreed to create that first draft) and then we can then collaborate on implementing/updating the document from there.
8. DID Portability
    1. Reviewed the spec concept
9. Potential conflict on the name "tdw"
    1. Project called "Trusted Digital Web" which has used the "TDW" acronym, although not for a DID Method. The project does mention DIDs. How to address the potential conflict? Michael Herman [raised an objection](https://github.com/w3c/did-extensions/pull/581#issuecomment-2462828639) on the DID Registration PR that we submitted.


## Meeting - 24 Oct 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording Link](https://us02web.zoom.us/rec/play/3FZV0hFVCnz8gr3ott015VG9Bcmns-WO4e6uG4pe2lUw8yOfOY4cO3Bkz-yv6qR_haZOK-tGx8Wj0KqY.BW8gH_WbxH9-lOOa?canPlayFromShare=true&from=share_recording_detail&continueMode=true&componentName=rec-play&originRequestUrl=https%3A%2F%2Fus02web.zoom.us%2Frec%2Fshare%2FghIRQB8gKWoypD7CCc6HFq1q_ELB0o4cVq-ogXUHZ16fiGlmU-XIOiyHbLYly1kd.TvBDukHqPBavC7CA)

Attendees:

- Stephen Curran
- And others...


1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
2. Latest spec updates and implementation notes.
    1. Cleaning up `[[spec]]` references -- Brian has enabled us to add our own spec references.
    2. Next up -- DRYing the. spec.
    3. Security and Privacy sections. Anyone able to help?
    4. Getting "spec to a standard" advice and applying those changes.
3. Registering did:tdw as a DID Method [PR](https://github.com/w3c/did-extensions/pull/581), and adding a did:tdw component to the [Universal Resolver](https://dev.uniresolver.io/).
4. [DID Linked Resources](https://w3c-ccg.github.io/DID-Linked-Resources/) and did:tdw
    1.  Mechanisms to publish/resolve files related to the DID -- e.g., AnonCreds objects, OCA Files, BitListStatus, etc.
    2.  DID Linked Resources vs. `relativeRef` [currently in the spec](https://identity.foundation/trustdidweb/#did-url-path-resolution-service)
    3.  Complexity of DID Linked Resources is that the DLRs must be listed somewhere so they can be included in the DID Metadata that is part of the DID resolution result.
        4.  Use case: Clients of resolvers find a DID URL for the resources. With `relativeRef` there is the same DID-To-HTTPS transformation to get the resource as to get the DID Log/DID Doc.
        5.  Use case: A resource points to a sequence of related documents, as in the case of RevRegEntries in AnonCreds. One identifier, but multiple resources. How does one find (a) the latest, (b) the entire list of entries (c) a specific entry at a given time? Each of those features could be needed with RevRegEntries.
4. [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb/issues)
    1.  Issues that would be [breaking changes](https://github.com/decentralized-identity/trustdidweb/issues?q=is%3Aissue+is%3Aopen+label%3Adiscuss) -- close them?
5. Update on the [did:tdw Web Server](https://github.com/decentralized-identity/trustdidweb-server-py) -- Patrick St. Louis.
6. Open Discussion

## Meeting - 10 Oct 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording Link](https://us02web.zoom.us/rec/share/6-GXLTdLVpl4RHBV91ekQtMovFphHokEcEJTRhPdaXgma3C-yd--XkNl7DNqcU0m.S3TQh10RxsfjBs8u)

Attendees:

- Stephen Curran
- Sylvain Martel

1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
2. Latest spec updates and implementation notes.
    1. Version 0.4 is out.
    2. [https://didtdw.org/](https://didtdw.org/) site is published.
    3. Implementer's Guide, etc. removed from the spec
    4. Next up -- DRYing the. spec.
    5. Anyone know a "Spec Veteran" that would be willing to review and point out deficiencies and potential improvements in the spec? Especially one with W3C spec experience.
        1. Suggestion to wait on this until after the DRYing is done.
3. Update on the [did:tdw Web Server](https://github.com/decentralized-identity/trustdidweb-server-py) -- Patrick St. Louis.
4. [DID Linked Resources](https://w3c-ccg.github.io/DID-Linked-Resources/) and did:tdw
    1. Should we? How?
5. [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb/issues)
6. Open Discussion

## Meeting - 26 Sept 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording Link](https://us02web.zoom.us/rec/share/VQ1FPX8yumZWnt7WVUYzvPgxcWA2WZZajGDbdDurqfc4T9V4VEEYrbmRiQoHcHc.DBkCFEu9ObvfsWg0)

Attendees:

- Stephen Curran
- Others...

1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
2. Feedback from implementing `did:tdw` Witness capability -- Brian Richter.
    1. Resolver has a /witness endpoint -- got the request from the DID Controller.
    2. Stuck on signing the entry. Both log entries have a did:key -- the witnesses must be published DIDs -- **SHOULD** be a did:tdw?
    3. Where to send the witness request? The DID Controller should know that. 
    4. Perhaps add an endpoint for the witnesses in the `witnesses` object? Decided no -- not to include the endpoint since that puts too much definition in the specification on how to implement the DID Controller and Witness interface. It is left to the DID Controller and witnesses to decide how they will interact. All that is specified is that  resolvers can verify the proofs via the DID referenced in the `witnesses` object, and the key identifier that references that DID in the proof itself.
    5. Use cases for witnesses -- (1) monitoring the DID controller to prevent maliciousness -- no backtracking, (2) preventing attacks on the DID Controller.
    6. Next steps -- Brian to continue implementing based on the discussion. Addition of weasel words to the spec to note the implementation challenges.
4. Spec. update to switch from a DID log entry being a JSON array to an object.  Feedback? -- Stephen Curran. Good to go with the names of the items in the object.
    1. General feedback -- all good.
    2. We reviewed the names and agreed with the ones in the PR now -- `versionId`, `versionTime` (both of which align with the DID Core spec query parameters), `parameters`, and `state`. `proof` is as defined in the DI specification.
6. Proof Chain vs. Proof Set
    1. Semantics:
        1. Proof Chain implies that that a subsequent signature is added to an existing signature, implying an attestation of that signature. But there are no implementations of it that we know of, and it's inclusion adds complexity without the semantics giving much benefit in `did:tdw`.
        2. Proof sets are just independent proofs across the same data.
    3. For now, let's just go with proof sets, as there is little benefit from using proof chains.
7. Update on the [did:tdw Web Server](https://github.com/decentralized-identity/trustdidweb-server-py) -- Patrick St. Louis.
    8. Demo given, but we ran out of time.
9. [DID Linked Resources](https://w3c-ccg.github.io/DID-Linked-Resources/) and did:tdw
    1. Should we?
10. [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb)
11. Open Discussion

## Meeting - 12 Sept 2024

Time: 9:00 Pacific / 18:00 Central Europe

Recording: [Zoom Recording Link](https://us02web.zoom.us/rec/share/-tz_UHUxqNxkGntAcYZEaYD1_EDX4RBVmtwd7JbGKFglgwTYzJUzkC2ZuFRFZftt.-wWj9ZMqS70V8Cwg)

Attendees:

* Stephen Curran
* Dmitri Zagidulin
* Cole Davis
* Brian Richter
* Andrew Whitehead
* Sylvain Martel
* Martina Kolpondinos
* John Jordan
* Patrick St. Louis
* Jamie Hale

1. Welcome and Adminstrivia
    1. Recording on?
    2. Please make sure you: [join DIF], [sign the WG Charter], and follow the [DIF Code of Conduct]. Questions? Please contact [operations@identity.foundation].
    3. [did:tdw Specification license] -- W3C Mode
    4. Introductions and Agenda Topics
2. Introduction to the `did:tdw` Work Item at DIF
    1. [CCG Presentation on did:tdw](https://meet.w3c-ccg.org/archives/w3c-ccg-weekly-2024-07-09.mp4) (starts at the 5:40 mark of recording)
3. Brief(!) introduction to `did:tdw`
4. Discussion:
    1. What do you want this group to achieve?
    2. What would help you the most?
        1. Get to 1.0!
        2. Web server
        3. Next step topics -- witnesses, deactivation -- how does a diploma remain verifiable when the isssuer disappears -- with their web server.  Aka durability.
        4. Acceptance of the method broadly.
        5. How `did:tdw` compares with KERI.
        6. Test suite!!!!! Implementation consistency.
        7. Cryptographic audit on the techniques used -- hashing use, etc.
        8. Governemnt acceptance of the cryptographic suites being used.
        9. Switchcord is running live use cases based on `did:web` -- would like to transition to `did:tdw` and its features.
5. Feature list [feedback document](https://docs.google.com/document/d/1jaKy0KbImVplW7xl2jMzI_MgayMQbI2o8TI6ZTvWHso/edit?usp=sharing) -- importance of features?
6. Future Topics
7. Next Meeting -- next week, same time
    1. Input to TPAC.
8. [Spec. PRs and Issues](https://github.com/decentralized-identity/trustdidweb)
9. Action items and next steps:
    1. Stephen to create a PR to change the spec. to say that a version is an object, JSON Patch is no longer used, and that the Data Integrity Proof is across the version object, without a challenge.
    2. Everyone to look at the [list of did:tdw features](https://docs.google.com/document/d/1jaKy0KbImVplW7xl2jMzI_MgayMQbI2o8TI6ZTvWHso/edit?usp=sharing) and comment on the features.
    3. Everyone to review issues and open others as needed, to drive future discussions.
