#!/usr/bin/env python

import os
import sys
from string import Template
import argparse
import hashlib

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Rename files based on content.')

    parser.add_argument('files', metavar='FILE', type=str, nargs='+',
                       help='Files to rename')
    parser.add_argument('-m', '--mask', nargs='?',
                       help='File destination mask', default='${hash}.${ext}')
    parser.add_argument('-p', '--pretend', action='store_true',
                       help='Do not rename, just print')

    args = parser.parse_args()
    print 'Renaming with mask: %s' % args.mask
    mask = Template(args.mask)
    for f in args.files:
        if not os.path.exists(f):
            print >>sys.stderr, 'File %s does not exists.' % f
        else:
            with open(f) as fp:
                h = hashlib.sha1(fp.read()).hexdigest()
            ext = os.path.splitext(f)[1][1:]
            name = mask.substitute(hash=h, ext=ext)
            dest = os.path.join(os.path.dirname(f), name)
            if os.path.basename(f) == name:
                print 'OK %s' % name
            else:
                print "`%s' -> `%s'" % (f, dest)
                if os.path.exists(dest):
                    print >>sys.stderr, 'Destination %s already exists.' % dest
                elif not args.pretend:
                    os.rename(f, dest)
