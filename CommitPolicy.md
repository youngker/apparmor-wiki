Userspace
---------

**TODO** Review and update policy for Gitlab merge requests.

**TODO** Propose a formatting rule of thumb for git commits.

-   Patches (for both trunk and minor releases) must be sent to the
    apparmor mailing list for review.
    -   If ACKed on the mailing list by at least 1 committer, they
        can be committed
        -   If no ACK in 1 week, senders (with commit rights) can
            commit and need to follow up to the list that the commit
            was made due to time-out.
        -   Patches from non-committers need to be merged/committed
            by the committer who ACKed the patch.
    -   If patch or proposed change is NAKed. It should be discussed
        and given due consideration, and new patches should be sent.
        -   If there is a NAK from a core dev, and an impass is reached
            (due perhaps to difference of opinion), then if 3 other
            core devs will ACK the change it will be accepted, and
            should be committed.

            If this situation is ever encounter due consideration must
            be given to the NAK and reasonable amount of effort should
            be taken to discuss/address the issue, but a NAK is not
            a veto and can be appealed and overriden where necessary.
    -   Exceptions to this policy:
        -   Changes to abstractions that are specific to a distro
            (e.g. profiles/apparmor.d/abstractions/ubuntu-\*) do not
            require ACKs for representatives/maintainers of those
            distros (e.g. jdstrand of Ubuntu).
        -   Translations done through launchpad translations are
            considered a patch submission, and can be merged after
            review, without needing to be forwarded to the list
            (unless questions arise, of course).

Kernel
------

-   Trunk commits are at the discretion of committers.
    -   Non-committers need to send patches to the mailing list
        for review.
        -   If ACKed on the mailing list by at least 1 committer,
            that committer is responsible for merging the commit
-   Stable release updates should be sent to the mailing list for
    review but committers are not required to wait for the standard
    1 week timeout.

------------------------------------------------------------------------

Note that these policies are just minimum requirements; submitters
and reviewers can request additional review.
