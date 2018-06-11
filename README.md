# NAME

Test::RequiredMinimumDependencyVersion - Require a minimum version for your dependencies

# VERSION

Version 0.001

# SYNOPSIS

    use Test::RequiredMinimumDependencyVersion;
    Test::RequiredMinimumDependencyVersion->new(module => { ... })->all_files_ok;

# DESCRIPTION

There are some modules where you'll always depend on a minimal version,
either because of a bug or because of an API change. A good example would be
[Test::More](https://metacpan.org/pod/Test::More) where version 0.88 introduced `done_testing()`.

This test can be used to check that, whenever you use these modules, you also
declare the minimum version.

This test is an author test and should not run on end-user installations.
Recommendation is to put it into your `xt` instead of your `t` directory.

# USAGE

## new( \[ ARGS \] )

Returns a new `Test::RequiredMinimumDependencyVersion` instance. `new`
takes a hash with its arguments.

    Test::RequiredMinimumDependencyVersion->new(
        module => {
            'Test::More' => '0.88',
        },
    );

The following arguments are supported:

### module (required)

The `module` argument is a hash ref where the keys are the modules you want
to enforce and the minimal version is its value.

## file\_ok( FILENAME )

This will run a test for parsing the file with
[Perl::PrereqScanner](https://metacpan.org/pod/Perl::PrereqScanner) and another test for every
`module` you specified if it is used in this file. It is therefore unlikely
to know the exact number of tests that will run in advance. Use
`done_testing` from [Test::More](https://metacpan.org/pod/Test::More) if you call this test directly
instead of a `plan`.

`file_ok` returns something _true_ if all web links are reachable
and _false_ otherwise.

## all\_files\_ok( \[ @entries \] )

Checks all the files under `@entries` by calling `pod_file_ok` on every
file. Directories are recursive searched for files. Everything not a file and
not a directory (e.g. a symlink) is ignored. It calls `done_testing` or
`skip_all` so you can't have already called `plan`.

If `@entries` is empty default directories are searched for files
containing Pod. The default directories are `blib`, or `lib` if it doesn't
exist, `bin` and `script`.

&lt;all\_files\_ok> returns something _true_ if all files test ok and _false_
otherwise.

# EXAMPLES

## Example 1 Default Usage

Check all files in the `bin`, `script` and `lib` directory.

    use 5.006;
    use strict;
    use warnings;

    use Test::RequiredMinimumDependencyVersion;

    Test::RequiredMinimumDependencyVersion->new(
        module => {
            'Test::More' => '0.88',
        },
    )->all_files_ok;

## Example 2 Check non-default directories or files

    use 5.006;
    use strict;
    use warnings;

    use Test::RequiredMinimumDependencyVersion;

    Test::RequiredMinimumDependencyVersion->new(
        module => {
            'Test::More' => '0.88',
        },
    )->all_pod_files_ok(qw(
        corpus/hello
        corpus/world.pl
        lib
        tools
    ));

## Example 3 Call `file_ok` directly

    use 5.006;
    use strict;
    use warnings;

    use Test::More 0.88;
    use Test::RequiredMinimumDependencyVersion;

    my $trmdv = Test::RequiredMinimumDependencyVersion->new(
        module => {
            'Test::More' => '0.88',
        },
    );
    $trmdv->pod_file_ok('corpus/7_links.pod');
    $trmdv->pod_file_ok('corpus/hello');

    done_testing();

head1 SEE ALSO

[Test::More](https://metacpan.org/pod/Test::More)

# SUPPORT

## Bugs / Feature Requests

Please report any bugs or feature requests through the issue tracker
at [https://github.com/skirmess/Test-RequiredMinimumDependencyVersion/issues](https://github.com/skirmess/Test-RequiredMinimumDependencyVersion/issues).
You will be notified automatically of any progress on your issue.

## Source Code

This is open source software. The code repository is available for
public review and contribution under the terms of the license.

[https://github.com/skirmess/Test-RequiredMinimumDependencyVersion](https://github.com/skirmess/Test-RequiredMinimumDependencyVersion)

    git clone https://github.com/skirmess/Test-RequiredMinimumDependencyVersion.git

# AUTHOR

Sven Kirmess <sven.kirmess@kzone.ch>

# COPYRIGHT AND LICENSE

This software is Copyright (c) 2018 by Sven Kirmess.

This is free software, licensed under:

    The (two-clause) FreeBSD License
