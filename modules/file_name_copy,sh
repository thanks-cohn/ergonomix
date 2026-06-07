#!/usr/bin/env bash
set -e

SERVICE_DIR="$HOME/.local/share/kio/servicemenus"

mkdir -p "$SERVICE_DIR"

need_wl_copy() {
  if ! command -v wl-copy >/dev/null 2>&1; then
    echo "Missing dependency: wl-copy"
    echo "Install it with:"
    echo "  sudo pacman -S wl-clipboard"
    exit 1
  fi
}

refresh_dolphin() {
  kbuildsycoca6 >/dev/null 2>&1 || true
  killall dolphin >/dev/null 2>&1 || true
}

install_copy_filenames() {
  need_wl_copy

  cat > "$SERVICE_DIR/ergonomix-copy-filenames.desktop" <<'EOF'
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=all/all;inode/directory;
Actions=copyFileNames;

[Desktop Action copyFileNames]
Name=Copy File Name(s)
Icon=edit-copy
Exec=/usr/bin/bash -c 'for f in "$@"; do basename "$f"; done | /usr/bin/wl-copy' dummy %F
EOF

  chmod +x "$SERVICE_DIR/ergonomix-copy-filenames.desktop"
}

install_copy_locations() {
  need_wl_copy

  cat > "$SERVICE_DIR/ergonomix-copy-locations.desktop" <<'EOF'
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=all/all;inode/directory;
Actions=copyFileLocations;

[Desktop Action copyFileLocations]
Name=Copy File Location(s)
Icon=edit-copy
Exec=/usr/bin/bash -c 'printf "%s\n" "$@" | /usr/bin/wl-copy' dummy %F
EOF

  chmod +x "$SERVICE_DIR/ergonomix-copy-locations.desktop"
}

install_oneup() {
  cat > "$SERVICE_DIR/ergonomix-oneup.desktop" <<'EOF'
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=all/all;
Actions=oneup;

[Desktop Action oneup]
Name=OneUp - Move One Folder Up
Icon=go-up
Exec=/usr/bin/bash -c 'for f in "$@"; do parent="$(dirname "$f")"; target="$(dirname "$parent")/$(basename "$f")"; mv -n "$f" "$target"; done' dummy %F
EOF

  chmod +x "$SERVICE_DIR/ergonomix-oneup.desktop"
}

install_sendhere() {
  cat > "$SERVICE_DIR/ergonomix-sendhere.desktop" <<'EOF'
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=inode/directory;
Actions=sendHere;

[Desktop Action sendHere]
Name=SendHere - Move Clipboard Files Here
Icon=folder-move
Exec=/usr/bin/bash -c 'dest="$1"; shift; /usr/bin/kdialog --msgbox "SendHere placeholder installed. Full file-picker version comes next."' dummy %f
EOF

  chmod +x "$SERVICE_DIR/ergonomix-sendhere.desktop"
}

install_all() {
  install_copy_filenames
  install_copy_locations
  install_oneup
  install_sendhere
  refresh_dolphin
}

case "${1:-menu}" in
  --all|all)
    install_all
    ;;
  copy-filenames)
    install_copy_filenames
    refresh_dolphin
    ;;
  copy-locations)
    install_copy_locations
    refresh_dolphin
    ;;
  oneup)
    install_oneup
    refresh_dolphin
    ;;
  sendhere)
    install_sendhere
    refresh_dolphin
    ;;
  *)
    echo "ERGONOMIX INSTALLER"
    echo
    echo "Usage:"
    echo "  ./ergonomix-install.sh --all"
    echo "  ./ergonomix-install.sh copy-filenames"
    echo "  ./ergonomix-install.sh copy-locations"
    echo "  ./ergonomix-install.sh oneup"
    echo "  ./ergonomix-install.sh sendhere"
    echo
    exit 0
    ;;
esac

echo "Installed Ergonomix module(s)."
echo "Right-click inside Dolphin to test."
