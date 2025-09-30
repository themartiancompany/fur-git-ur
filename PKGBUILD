# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2023, 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>


if [[ ! -v "_evmfs" ]]; then
  _evmfs="false"
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_docs" ]]; then
  _docs="false"
fi
if [[ ! -v "_offline" ]]; then
  _offline="false"
fi
# _local=false
_py="python"
_proj="hip"
_pkg=fur
pkgbase="${_pkg}-git"
pkgname=(
  "${_pkg}-git"
)
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${_pkg}-docs-git"
  )
fi
pkgver="0.0.0.1.1.1.1.r18.gfac5075dca616595ca35b9d562a1036a94a9f521"
pkgrel=1
_pkgdesc=(
  "Ur helper."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_gl="gitlab.com"
_gh="github.com"
_host="https://${_gh}"
_ns='themartiancompany'
_local="${HOME}/${_pkg}"
url="${_host}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkg}"
license=(
  "AGPL3"
)
depends=(
  "git"
  "libcrash-bash"
)
makedepends=(
  "make"
)
if [[ "${_docs}" == "true" ]]; then
  makedepends+=(
    "${_py}-docutils"
  )
fi
checkdepends=(
  "shellcheck"
)
optdepends=(
)
provides=(
  "${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkg}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
if [[ "${_offline}" == true ]]; then
  _url="${_local}"
fi
source=()
_branch="main"
_tarname="${_pkg}-${_branch}"
if [[ "${_git}" == true ]]; then
  makedepends+=(
    'git'
  )
  source+=(
    "${_tarname}::git+${_url}#branch=${_branch}"
  )
elif [[ "${_git}" == false ]]; then
  makedepends+=(
    "curl"
    "jq"
  )
  source+=(
    "${_tarname}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
fi
sha256sums=(
  "SKIP"
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F \
        '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_tarname}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

package_fur-git() {
  local \
    _make_opts=()
  _make_opts+=(
    DESTDIR="${pkgdir}"
    PREFIX="/usr"
    VERBOSE="true"
  )
  cd \
    "${_tarname}"
  make \
    "${_make_opts[@]}" \
    install-fur
}

package_fur-docs-git() {
  local \
    _make_opts=()
  _make_opts+=(
    DESTDIR="${pkgdir}"
    PREFIX="/usr"
    VERBOSE="true"
  )
  cd \
    "${_tarname}"
  make \
    "${_make_opts[@]}" \
    install-doc \
    install-man
}

# vim: ft=sh syn=sh et
