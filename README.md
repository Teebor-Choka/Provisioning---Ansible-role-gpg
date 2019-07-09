Ansible role: GPG
=========

Manage GPG keys and trusts.

Requirements
------------

None

Role Variables
--------------

These defaults are set in defaults/main.yml:

    hide_secrets: true      # omit key details from logs

Other variables that can be set:

    generate:
      keys: <list, key parameters to be used for generating>
        - user: <string>
          email: <string, e-mail associated with this key>
          type: <string, key type (e.g. RSA)>
          length: <int>
          passphrase: <string, passphrase used to unlock the key>
          expiration: <date, when should the key expire (e.g. 0 for never)>
          sub_keys:
            - purpose: <string, subkey type (e.g. sign/encrypt)>
              length: <int, subkey length>
              expiration: <date, when should the subkey expire (e.g. 0 for never)>

    import:
      trusts: <list, trusts to import>
        - path: <string, path to trust>
      keys: <list, keys to import>
        - public: <string, path to public key>
          private: <string, path to private key>
          passphrase: <string, path to passphrase>

    export:
      dir: <string, destination for local trust file export>

      keys: <list, args for key exports>
        - email: <string>
          passphrase: <string, passphrase used to unlock the key>

      export_trust: <bool, should the trust be exported>
      harden: <bool, harden by removing the private master, leaving behind only subkeys>


Dependencies
------------

None

Example Playbook
----------------

Specify the parameters needed for GPG key manipulation.

    - hosts: localhost
      roles:
        - role: gpg
          vars:
            hide_secrets: yes
            generate:
              keys:
                - user: test
                  email: test@test.com
                  passphrase: test
                  comment: "No comment is not the best comment."
                  sub_keys:
                    - purpose: 'sign'
                      length: 2048

            export:
              dir: ~/test-export
              export_trust: yes
              harden: yes
              keys:
                - email: test@test.com
                  passphrase: test-passphrase-to-allow-export

            import:
              trusts:
                - path: "/path/to/trust.pgp"
              keys:
                - public: ~/public.key
                  private: ~/private.key
                  passphrase: test


License
-------

MIT

Author Information
------------------

Tibor Csóka
