
# EDA Tools Automated Setup Script
# Tools: Yosys, Iverilog, GTKWave

set -e  # Exit on error

echo "🔧 EDA Tools Setup Starting..."
echo "================================"

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Check if running as root
if [[ $EUID -eq 0 ]]; then
   echo -e "${RED}Don't run this as root!${NC}" 
   exit 1
fi

echo -e "${BLUE}Step 1: Updating system...${NC}"
sudo apt update && sudo apt upgrade -y

echo -e "${BLUE}Step 2: Installing build tools...${NC}"
sudo apt install -y git build-essential cmake wget curl

echo -e "${BLUE}Step 3: Installing Yosys dependencies...${NC}"
sudo apt install -y clang bison flex libreadline-dev gawk \
    tcl-dev libffi-dev graphviz xdot pkg-config python3 \
    libboost-system-dev libboost-python-dev \
    libboost-filesystem-dev zlib1g-dev

echo -e "${BLUE}Step 4: Building Yosys...${NC}"
if [ ! -d "yosys" ]; then
    git clone https://github.com/YosysHQ/yosys.git
fi
cd yosys
make config-clang
make -j$(nproc)
sudo make install
cd ..

echo -e "${BLUE}Step 5: Installing Iverilog...${NC}"
sudo apt install -y iverilog

echo -e "${BLUE}Step 6: Installing GTKWave...${NC}"
sudo apt install -y gtkwave

echo -e "${BLUE}Step 7: Testing installations...${NC}"

# Test Yosys
if yosys -V &>/dev/null; then
    echo -e "${GREEN}✅ Yosys installed successfully${NC}"
else
    echo -e "${RED}❌ Yosys installation failed${NC}"
fi

# Test Iverilog  
if iverilog -v &>/dev/null; then
    echo -e "${GREEN}✅ Iverilog installed successfully${NC}"
else
    echo -e "${RED}❌ Iverilog installation failed${NC}"
fi

# Test GTKWave
if gtkwave --version &>/dev/null; then
    echo -e "${GREEN}✅ GTKWave installed successfully${NC}"
else
    echo -e "${RED}❌ GTKWave installation failed${NC}"
fi

echo "================================"
echo -e "${GREEN}🎉 Setup completed!${NC}"
echo ""
echo "Quick test commands:"
echo "  yosys -V"
echo "  iverilog -v" 
echo "  gtkwave --version"
echo ""
echo "Ready for RISC-V development! 🚀"
